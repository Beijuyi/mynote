#springboot 


## Run ()
```java
public ConfigurableApplicationContext run(String... args) {  
    Startup startup = Startup.create();  
    //注册关闭钩子,确保应用程序正常退出时可以执行清理工作
    if (this.registerShutdownHook) {  
       SpringApplication.shutdownHook.enableShutdownHookAddition();  
    }  
    //创建引导上下文,该上下文在应用程序启动过程中提供必要的配置信息。
    DefaultBootstrapContext bootstrapContext = createBootstrapContext();  
    ConfigurableApplicationContext context = null;  
    //配置Java的无头模式属性，这影响图形化组件是否可用。
    configureHeadlessProperty();  
    //获取运行监听器
    SpringApplicationRunListeners listeners = getRunListeners(args);  
    //监听器启动,通知监听器应用程序启动
    listeners.starting(bootstrapContext, this.mainApplicationClass);  
    try {  
    //解析命令行参数
       ApplicationArguments applicationArguments = new DefaultApplicationArguments(args);  
       //准备应用程序的环境配置
       ConfigurableEnvironment environment = prepareEnvironment(listeners, bootstrapContext, applicationArguments);  
       //打印banner
       Banner printedBanner = printBanner(environment);  
       //创建应用上下文
       context = createApplicationContext();  
       context.setApplicationStartup(this.applicationStartup);  
       prepareContext(bootstrapContext, context, environment, listeners, applicationArguments, printedBanner);  
       refreshContext(context);  
       afterRefresh(context, applicationArguments);  
       startup.started();  
       if (this.logStartupInfo) {  
          new StartupInfoLogger(this.mainApplicationClass).logStarted(getApplicationLog(), startup);  
       }  
       listeners.started(context, startup.timeTakenToStarted());  
       callRunners(context, applicationArguments);  
    }  
    catch (Throwable ex) {  
       throw handleRunFailure(context, ex, listeners);  
    }  
    try {  
       if (context.isRunning()) {  
          listeners.ready(context, startup.ready());  
       }  
    }  
    catch (Throwable ex) {  
       throw handleRunFailure(context, ex, null);  
    }  
    return context;  
}
```