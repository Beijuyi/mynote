#springboot #web #mvc #自动装配

> [!参考文档]
> https://blog.csdn.net/qq_36942720/article/details/135218205


# WEB MVC 是什么?

模型-视图-控制器组件


# 入口
在 springboot 3 x 版本自动装配包 spring-boot-autoconfigure中的/META-INF/spring 目录下有一个文件: `org.Springframework.Boot.Autoconfigure.AutoConfiguration.Imports`, 该文件记录了 springboot 启动时会自动装配的配置类和条件.
其中**WebMvcAutoConfiguration** 配置类包含了web 自动装配的逻辑
![](Pasted%20image%2020250117131914.png)

先看WebMvcAutoConfiguration 上的注解
```java
@AutoConfiguration(after = { DispatcherServletAutoConfiguration.class, TaskExecutionAutoConfiguration.class,  
       ValidationAutoConfiguration.class })  
//表示只有项目是一个web项目,并且是SERVLET项目时才会配置
@ConditionalOnWebApplication(type = Type.SERVLET)  
//当项目类路径下有下面三个类时
@ConditionalOnClass({ Servlet.class, DispatcherServlet.class, WebMvcConfigurer.class })  
//当spring容器中没有WebMvcConfigurationSupport的bean对象时
@ConditionalOnMissingBean(WebMvcConfigurationSupport.class)  
//自动装配顺序
@AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE + 10)  
//引入了运行时提示,与 Web 资源的运行时行为有关
@ImportRuntimeHints(WebResourcesRuntimeHints.class)
public class WebMvcAutoConfiguration {
```

## @AutoConfiguration
这个注解用于告诉 springboot 自动装配的顺序.
总结一下这个注解封装了 `@AutoConfigureBefore` `@AutoConfigureAfter`, 分别用 before 和 after 属性来表示
`@AutoConfigureBefore` : 假如 A 配置类上有这个注解, 且注解中的值为 B 配置类, 那么表示 A 在 B 配置类之前先装配. 
`@AutoConfigureAfter`: 与 before 相反

这个自动配置类 `WebMvcAutoConfiguration` 主要负责在满足一系列条件的情况下配置 Spring MVC相关的组件，确保这些配置仅在特定的 Web 应用环境下生效.

WebMvcAutoConfiguration 中的各个方法作用
```java
@Configuration
@ConditionalOnWebApplication(type = Type.SERVLET)
@ConditionalOnClass({ Servlet.class, DispatcherServlet.class, WebMvcConfigurer.class })
@ConditionalOnMissingBean(WebMvcConfigurationSupport.class)
@EnableConfigurationProperties({ WebMvcProperties.class, ResourceProperties.class })
@Order(0)
public class WebMvcAutoConfiguration {

    @Configuration
    @Import(EnableWebMvcConfiguration.class)
    @EnableConfigurationProperties({ WebMvcProperties.class, ResourceProperties.class })
    public static class WebMvcAutoConfigurationAdapter implements WebMvcConfigurer {

        @Override
        public void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
            // 配置消息转换器
        }

        @Override
        public void configurePathMatch(PathMatchConfigurer configurer) {
            // 配置路径匹配
        }

        @Override
        public void configureContentNegotiation(ContentNegotiationConfigurer configurer) {
            // 配置内容协商
        }

        @Override
        public void configureAsyncSupport(AsyncSupportConfigurer configurer) {
            // 配置异步支持
        }

        @Override
        public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
            // 配置默认 Servlet 处理
        }

        @Override
        public void addFormatters(FormatterRegistry registry) {
            // 添加格式化器
        }

        @Override
        public void addInterceptors(InterceptorRegistry registry) {
            // 添加拦截器
        }

        @Override
        public void addResourceHandlers(ResourceHandlerRegistry registry) {
            // 添加资源处理器
        }

        @Override
        public void addCorsMappings(CorsRegistry registry) {
            // 添加 CORS 映射
        }

        @Override
        public void addViewControllers(ViewControllerRegistry registry) {
            // 添加视图控制器
        }

        @Override
        public void configureViewResolvers(ViewResolverRegistry registry) {
            // 配置视图解析器
        }

        @Override
        public void addArgumentResolvers(List<HandlerMethodArgumentResolver> resolvers) {
            // 添加参数解析器
        }

        @Override
        public void addReturnValueHandlers(List<HandlerMethodReturnValueHandler> handlers) {
            // 添加返回值处理器
        }

        @Override
        public void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
            // 配置消息转换器
        }

        @Override
        public void extendMessageConverters(List<HttpMessageConverter<?>> converters) {
            // 扩展消息转换器
        }

        @Override
        public void configureHandlerExceptionResolvers(List<HandlerExceptionResolver> resolvers) {
            // 配置异常解析器
        }

        @Override
        public void extendHandlerExceptionResolvers(List<HandlerExceptionResolver> resolvers) {
            // 扩展异常解析器
        }

        @Override
        public Validator getValidator() {
            // 获取验证器
            return null;
        }

        @Override
        public MessageCodesResolver getMessageCodesResolver() {
            // 
```