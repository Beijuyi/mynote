#代理 #reflect #反射

# Proxy 介绍

## newProxyInstance 方法

- 获取构造器
```java
@CallerSensitive  
public static Object newProxyInstance(ClassLoader loader,  
                                      Class<?>[] interfaces,  
                                      InvocationHandler h) {  
    Objects.requireNonNull(h);  
    @SuppressWarnings("removal")  
    final Class<?> caller = System.getSecurityManager() == null  
                                ? null  
                                : Reflection.getCallerClass();                     
    /*  
     * Look up or generate the designated proxy class and its constructor.     */
    //获取构造器 
    Constructor<?> cons = getProxyConstructor(caller, loader, interfaces);  
    return newProxyInstance(caller, cons, h);  
}
```
![](Pasted%20image%2020250116130901.png)
代理类名称: `org.Example.Aop.$Proxy0`
构造器: `public org.example.aop.$Proxy0(java.lang.reflect.InvocationHandler)`
- 构造器对象反射创建代理对象
```java
cons.newInstance(new Object[]{h})
```
- 检查类型不能是枚举类, **不能通过反射创建枚举对象**
- 创建或复用**构造函数访问器**, 作用是为了反射调用类的构造方法
- 会通过NativeConstructorAccessorImpl调用 native 方法创建实例
```java
private static native Object newInstance0(Constructor<?> c, Object[] args)  
    throws InstantiationException,  
           IllegalArgumentException,  
           InvocationTargetException;
```
![](Pasted%20image%2020250116133519.png)

![](Pasted%20image%2020250116133916.png)
创建的对象 `o = {$Proxy0@896}org.example.aop.UserDao@9807454`
`{$Proxy0@896}` : 表示代理类, @896 是对象的 hash 计算值
`org.example.aop.UserDao@9807454`: 表示代理对象实现了 UserDao 接口, @9807454 也表示 hash 计算值

