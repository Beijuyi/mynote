#java #aop #spring

Springboot 3.4. 1提供了 AOP 自动配置, 默认使用 CGLib 动态代理实现 AOP, 如果要使用 JDK 动态代理, 可以配置: ` spring.aop.proxy-target-class = false `
如果AspectJ 在类路径下, springboot 自动配置会自动开启支持 AspectJ auto proxy, 不需要显示声明 `@EnableAspectJAutoProxy` 
Spring 中关于 aop 的使用[Spring AOP APIs :: Spring Framework](https://docs.spring.io/spring-framework/reference/core/aop-api.html)), 对于一般项目, 推荐使用切点 pointcut.

# Pointcut 切点

切入点的使用
- AspectJ pointcut expressions 切入点表达式
- Union 并集
- Intersection 交集

> [Tips]
> Spring 推荐使用静态的pointcuts ,可以让 aop 代理创建时**缓存**切入点计算的结果


## 几种便利的切点实现

### Static Pointcuts静态切点

### Regular Expression Pointcuts 正则表达式切点

# AspectJ pointcut expressions 切入点表达式

