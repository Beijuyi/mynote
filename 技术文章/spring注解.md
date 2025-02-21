#注解 #spring #springboot 

# @Configurable
[详解Spring注解@Configurable](https://blog.ksisn.com/2020/12/20/%E8%AF%A6%E8%A7%A3Spring%E6%B3%A8%E8%A7%A3-Configurable/)
@Configurable 平时用的却较少，它用于解决非Spring容器管理的Bean中却依赖Spring Bean的场景，也就是说Bean A依赖了一个Spring的Bean B，但是A不是Spring 得Bean所以无法进行属性注入拿不到B的情况。
**关键点: 在一个类上加@Configurable 注解修饰, 并不能让这个类被 spring 容器管理.**
这是@Configurable 与@configuration 和@Component 注解之间的区别.



# 自动装配注解
## @ConditionalOnClass


## @ConditionalOnProperty



## @AutoConfigureAfter
该注解作用于类上, 如果自定义的自动配置类 A, 希望在另一个类 B之后配置, 就可以使用该注解
```java
@AutoConfigureAfter(value={"B全限定类名"})
public class A {
}
```

## @AutoConfigureBefore
与@AutoConfigureAfter 注解作用相反

## @Conditional
Spring 框架中的注解,允许根据特定条件动态地控制bean的创建和应用。通过使用这个注解，你可以基于某些条件决定是否应该加载某个配置类或注册某个bean.
`@Conditional` 注解本身需要与实现了 `Condition` 接口的类一起使用。`Condition` 接口包含了一个 `matches` 方法，该方法返回一个布尔值，用来指示给定的条件是否满足。如果 `matches` 方法返回 `true`，则对应的bean或配置将会被加载；否则，将被忽略。


# 依赖注入
## @Autowired 与@Resource

@Autowired 和 @Resource 都是 Spring/Spring Boot 项目中，用来进行依赖注入的注解.
[面试突击78：@Autowired 和 @Resource 有什么区别？-阿里云开发者社区](https://developer.aliyun.com/article/1003903)

- @Autowired 和 @Resource 都是既使用了名称查找又使用了类型查找，但二者进行查找的顺序却截然相反。

**@Autowired 是先根据类型（byType）查找，如果存在多个 Bean 再根据名称（byName）进行查找**
**@Resource 是先根据名称查找，如果（根据名称）查找不到，再根据类型进行查找**

