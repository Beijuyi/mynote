# 介绍

什么是注解?

为什么?

注解如何使用?

# 元注解

## @Target

用于规定注解使用的作用域,作用域是枚举类,有如下属性:

注意下面注解出现的时间有所不同.

```
public enum ElementType {
    /** Class, interface (including annotation interface), enum, or record
     * declaration */
    TYPE,//类,接口,注解,枚举和record声明

    /** Field declaration (includes enum constants) */
    FIELD,//属性,包括枚举常量

    /** Method declaration */
    METHOD,//方法

    /** Formal parameter declaration */
    PARAMETER,//方法形参或构造器形参

    /** Constructor declaration */
    CONSTRUCTOR,//构造器

    /** Local variable declaration */
    LOCAL_VARIABLE,//局部变量

    /** Annotation interface declaration (Formerly known as an annotation type.) */
    ANNOTATION_TYPE,//作用于注解,那么说明它是一个元注解

    /** Package declaration */
    PACKAGE,//包级注解

    /**
     * Type parameter declaration
     *
     * @since 1.8
     */
    TYPE_PARAMETER,//类型参数,泛型中用的类型变量

    /**
     * Use of a type
     *
     * @since 1.8
     */
    TYPE_USE,//作用于类型使用的任何位置

    /**
     * Module declaration.
     *
     * @since 9
     */
    MODULE,//作用于模块声明

    /**
     * Record component
     *
     * @jls 8.10.3 Record Members
     * @jls 9.7.4 Where Annotations May Appear
     *
     * @since 16
     */
    RECORD_COMPONENT;//作用于记录类(Record类)的组件(字段)
}
```