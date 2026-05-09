#javase #java 


## Protected 关键字

Protected 是 java 中的权限修饰符, 规范类中方法或属性的访问权限. 其他 3 个关键字好理解, protected 不好理解.
Protected 的可见范围是: 类内部, 本包, 子类
主要是第三个子类范围是什么意思?
子类范围表示的是: 不在本包内, 也就是外部包, 但是继承了本类, 本类中的被 protected 修饰的属性和方法, 对子类可见, 对其他外部类不可见.
```java
public class Dog {  
    private int id;  
    private String name;  
    // Getters and Setters  
    protected int getId() {  
        return id;  
    }  
    private void setId(int id) {  
        this.id = id;  
    }  
    private String getName() {  
        return name;  
    }  
    private void setName(String name) {  
        this.name = name;  
    }  
  
}
```
```java
public class Teddy extends Dog {  
    private int idd;  
    public static void main(String[] args) {  
        Teddy teddy = new Teddy();  //必须要
        int id = teddy.getId();  //可以使用被protected修饰的方法,无法使用private方法 
        Dog dog = new Dog();  
        dog.get  //无法使用自己的getId()方法
    }  
}
    }  
}
```
```java
public class Main {  
    public static void main(String[] args) {  
        Teddy teddy = new Teddy();  
        teddy.get //无法使用getId()方法, 因为可见性限制
        Dog dog = new Dog();  
        dog.get //无法使用getId()方法,因为可见性限制
    }  
}
```
Teddy 类继承 Dog 类, Dog 类中 getId () 方法被 protected 修饰, 其他方法都是 private.
Teddy 类和 Dog 不在同一个包下, 在 Teddy 中 Dog 的 protected 方法对其可见.
Main 和 Dog 也不在同一包下, 因此 Dog 中的 protected 方法对 Main 不可见, 无法使用.
