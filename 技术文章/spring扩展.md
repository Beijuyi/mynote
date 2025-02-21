#spring 


# WebMvcConfigurer
通过实现 WebMvcConfigurer 接口来自定义 web 配置类, 可以扩展一些 web配置.

# HandlerInterceptor
通过实现 WebMvcConfigurer 接口来自定义拦截器, 然后在上面的自定义 web 配置类添加到请求处理流程中.

# Filter
通过实现 Filter 接口来自定义过滤器, 然后通过配置注入请求处理流程中.
也可以通过 Filter 的子接口和抽象类来实现.

例如下面 MDCFilter, 给日志增加 trace-id
```java
public class MDCFilter extends OncePerRequestFilter {  
    private static final String TRACE_ID = "trace-id";  
    @Override  
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws ServletException, IOException {  
        try {  
            String traceId = getTraceIdFromRequest(request);  
            if (traceId != null && !traceId.isEmpty()) {  
                MDC.put(TRACE_ID, traceId);  
            } else {  
                MDC.put(TRACE_ID, generateTraceId());  
            }  
  
            response.setHeader("X-Trace-ID", MDC.get(TRACE_ID));  
            filterChain.doFilter(request, response);  
  
        } finally {  
            MDC.clear();  
        }  
    }  
  
    private String getTraceIdFromRequest(HttpServletRequest request) {  
        return request.getHeader("X-Trace-ID");  
    }  
  
    private String generateTraceId() {  
        return java.util.UUID.randomUUID().toString();  
    }  
}
```

```java
@Configuration  
public class MDCFilterConfig {  
  
    @Bean  
    public FilterRegistrationBean<MDCFilter> loggingMDCFilter() {  
        FilterRegistrationBean<MDCFilter> registrationBean = new FilterRegistrationBean<MDCFilter>();  
        registrationBean.setFilter(new MDCFilter());  
        registrationBean.setOrder(1); // 值越小,优先级越高  
        return registrationBean;  
    }  
}
```

OncePerRequestFilter
# PropertySourcesPlaceholderConfigurer

下面这个例子, 自定义了一个工具类 ConfigUtil, 将 propertyResolver 对象注入到 ConfigUtil 中, 通过 ConfigUtil 借助 `propertyResolver.getProperty(key)` 的能力获取配置的值
```java
@Configuration  
@Slf4j  
public class PropertyConfig extends PropertySourcesPlaceholderConfigurer{  
  
    @Override  
    protected void processProperties(@Nonnull ConfigurableListableBeanFactory beanFactoryToProcess,  
                                     @Nonnull ConfigurablePropertyResolver propertyResolver) throws BeansException {  
        super.processProperties(beanFactoryToProcess, propertyResolver);  
        log.debug("placeHolderConfigurer init");  
        ConfigUtil.setConfig(propertyResolver);  
    }  
}
```

