#持久层 #mybatis

> [!文档]
> [MyBatis中文网](https://mybatis.net.cn/)
> [Wch](https://wch853.github.io/posts/mybatis/MyBatis%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90%EF%BC%88%E4%B8%80%EF%BC%89%EF%BC%9AMyBatis%E7%AE%80%E4%BB%8B%E5%92%8C%E6%95%B4%E4%BD%93%E6%9E%B6%E6%9E%84.html#mybatis-%E7%AE%80%E4%BB%8B)
> 

# 简介

 **MyBatis** 是一款优秀的持久层框架，它支持自定义SQL、存储过程以及高级映射。
 MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。
 MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。

# 核心概念

### JDBC
Java数据库连接 Java Database Connectivity，简称JDBC，它是Java语言中用来规范客户端程序如何来访问数据库的应用程序接口，提供了诸如查询和更新数据库中数据的方法。
JDBC是一类接口，制定了统一访问各类关系数据库的标准接口。
JDBC是接口，驱动是接口的实现类，没有驱动将无法完成数据库连接，从而不能操作数据库！


### 原理
![](Pasted%20image%2020250226192726.png)

（1）读取MyBatis的配置文件。mybatis-config.xml为MyBatis的全局配置文件，用于配置数据库连接信息。

（2）加载映射文件。映射文件即SQL映射文件，该文件中配置了操作数据库的SQL语句，需要在MyBatis配置文件mybatis-config.xml中加载。mybatis-config.xml 文件可以加载多个映射文件，每个文件对应数据库中的一张表。

（3）构造会话工厂。通过MyBatis的环境配置信息构建会话工厂SqlSessionFactory。

（4）创建会话对象。由会话工厂创建SqlSession对象，该对象中包含了执行SQL语句的所有方法。

（5）Executor执行器。MyBatis底层定义了一个Executor接口来操作数据库，它将根据SqlSession传递的参数动态地生成需要执行的SQL语句，同时负责查询缓存的维护。

（6）MappedStatement对象。在Executor接口的执行方法中有一个MappedStatement类型的参数，该参数是对映射信息的封装，用于存储要映射的SQL语句的id、参数等信息。

（7）输入参数映射。输入参数类型可以是Map、List等集合类型，也可以是基本数据类型和POJO类型。输入参数映射过程类似于JDBC对preparedStatement对象设置参数的过程。

（8）输出结果映射。输出结果类型可以是Map、List等集合类型，也可以是基本数据类型和POJO类型。输出结果映射过程类似于JDBC对结果集的解析过程。
## SqlSessionFactory

每个基于 MyBatis 的应用都是以一个 SqlSessionFactory 的实例为核心的。
SqlSessionFactory 的实例可以通过 SqlSessionFactoryBuilder 获得。
 SqlSessionFactoryBuilder 则可以从 XML 配置文件或一个预先配置的 Configuration 实例来构建出 SqlSessionFactory 实例。
### XML 配置创建 SqlSessionFactory




# 源码

## SqlSessionFactoryBuilder

```java
public class SqlSessionFactoryBuilder {  
    public SqlSessionFactoryBuilder() {  
    }  
  //筛选了关键方法
    public SqlSessionFactory build(Reader reader, String environment, Properties properties) {  
        SqlSessionFactory var5;  
        try {  
            XMLConfigBuilder parser = new XMLConfigBuilder(reader, environment, properties);  
            var5 = this.build(parser.parse());  
        } catch (Exception var14) {  
            throw ExceptionFactory.wrapException("Error building SqlSession.", var14);  
        } finally {  
            ErrorContext.instance().reset();  
  
            try {  
                if (reader != null) {  
                    reader.close();  
                }  
            } catch (IOException var13) {  
            }  
  
        }  
  
        return var5;  
    }  
  
  
    public SqlSessionFactory build(InputStream inputStream, String environment, Properties properties) {  
        SqlSessionFactory var5;  
        try {  
            XMLConfigBuilder parser = new XMLConfigBuilder(inputStream, environment, properties);  
            var5 = this.build(parser.parse());  
        } catch (Exception var14) {  
            throw ExceptionFactory.wrapException("Error building SqlSession.", var14);  
        } finally {  
            ErrorContext.instance().reset();  
  
            try {  
                if (inputStream != null) {  
                    inputStream.close();  
                }  
            } catch (IOException var13) {  
            }  
  
        }  
  
        return var5;  
    }  
  
    public SqlSessionFactory build(Configuration config) {  
        return new DefaultSqlSessionFactory(config);  
    }  
}
```

## XPathParser && XPath

MyBatis使用XPath技术解析XML文档，MyBatis提供的**XPathParser**类对XPath的进行了封装，提供了更为友好的方法以方便用户使用。

XPathParser
```
public class XPathParser {  
	//属性
    private final Document document;  //表示一个Dom文档对象,通过解析xml文档生成
    private boolean validation;  //是否启用xml验证
    private EntityResolver entityResolver;  //此字段用于定义如何解析XML中的实体引用
    private Properties variables;  //这个字段存储了一组键值对属性，通常用于替换XML配置文件中的变量占位符。
    private XPath xpath; //用于执行XPath表达式查询，以定位和提取document中特定的节点或值。
    //构造器
    
    
}
```

XPath 语法: https://www.runoob.com/xpath/xpath-syntax.html


# 核心问题

核心问题清单:
- Mybatis 执行数据库操作的核心是SqlSession 根据 Mapper 接口获取的代理类对象来执行的, 代理类对象调用方法时增强了什么,?怎么实现的?
- 




Mybatis 中如何将 XML 中的语句与 Mapper 类中的方法映射起来的?
让 xml 文件跟 mapper 类对应的已知的几个原则:
- Namespace: XML 映射文件中的 `namespace` 属性值必须与 Mapper 接口的全限定名一致
- Id: XML 文件中 SQL 语句的 `id` 值要与接口中的方法名保持一致
就这两个要求.

MyBatis 使用动态代理模式为每个 Mapper 接口创建代理对象。
当你调用 `UserMapper` 中的方法时，实际上调用的是 MyBatis 生成的代理对象。这个代理对象会根据方法名查找与之关联的 SQL 语句，并将参数传入到 SQL 中执行，最后返回结果。
```java
//这里创建的对象是代理对象,不是我们写的UserMapper对象 //org.apache.ibatis.binding.MapperProxy@3bd94634
UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
User user = userMapper.selectUserById(1);
```


关联过程
1. MyBatis 通过 `MapperRegistry` 注册 Mapper 接口。
2. 当调用 `getMapper` 方法时，`MapperProxyFactory` 生成相应的代理对象。
3. 代理对象通过 `MapperProxy` 调用对应的 SQL 语句。
MapperRegistry
```java

```

MapperProxyFactory
```java

```

MapperProxy
```java

```


## Mybatis 生成的代理对象如何执行数据库操作的?

```java
public class MapperProxy<T> implements InvocationHandler, Serializable {

	//构造器
	public MapperProxy(SqlSession sqlSession, Class<T> mapperInterface, Map<Method, MapperMethodInvoker> methodCache) {  
    this.sqlSession = sqlSession;  
    this.mapperInterface = mapperInterface;  
    this.methodCache = methodCache;  
}

	//重写invoke方法
	public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {  
    try {  
        return Object.class.equals(method.getDeclaringClass()) ? method.invoke(this, args) : this.cachedInvoker(method).invoke(proxy, method, args, this.sqlSession);  
    } catch (Throwable var5) {  
        throw ExceptionUtil.unwrapThrowable(var5);  
    }  
}

	private static class DefaultMethodInvoker implements MapperMethodInvoker {  
    private final MethodHandle methodHandle;  
  
    public DefaultMethodInvoker(MethodHandle methodHandle) {  
        this.methodHandle = methodHandle;  
    }  
  
    public Object invoke(Object proxy, Method method, Object[] args, SqlSession sqlSession) throws Throwable {  
        return this.methodHandle.bindTo(proxy).invokeWithArguments(args);  
    }  
}


	private static class PlainMethodInvoker implements MapperMethodInvoker {  
	    private final MapperMethod mapperMethod;  
	  
	    public PlainMethodInvoker(MapperMethod mapperMethod) {  
	        this.mapperMethod = mapperMethod;  
	    }  
  
	    public Object invoke(Object proxy, Method method, Object[] args, SqlSession sqlSession) throws Throwable {  
	        return this.mapperMethod.execute(sqlSession, args);  
	    }  
}

	interface MapperMethodInvoker {  
	    Object invoke(Object proxy, Method method, Object[] args, SqlSession sqlSession) throws Throwable;  
	}
}
```


# Mybatis 插件
https://houbb.github.io/2019/01/23/mybatis-inteceptor

Mybatis 定义的Interceptor 接口
```java
public interface Interceptor {  
  
  Object intercept(Invocation invocation) throws Throwable;  
  
  default Object plugin(Object target) {  
    return Plugin.wrap(target, this);  
  }  
  
  default void setProperties(Properties properties) {  
    // NOP  
  }  
  
}
```
