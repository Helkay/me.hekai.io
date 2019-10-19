---
title: Java 注解 Annotations
categories:
  - Blog
tags:
  - Java
abbrlink: 50788
date: 2017-05-11 00:00:00
---

### 什么是注解？
  可理解为元数据，即一种描述数据的数据。
  
``` java
@Override
public String toString() {
    return "toString...";
}
```
在上面的代码中我用Override重写了toString()代码,当然即使我不用注解,程序也是能够正常运行的,但加上了Override注解,它会告诉编译器这是个重写的方法,如果我不小心写错了方法名,而父类中没有该方法,则编译器就会报错。
  
### 有关Annotation的一些说明
java8 版本在 java.lang.annotation提供了六种元注解(Native和Repeatable是java8加入的，其他的是java5),用来注解其他注解:

#### @Documented	: 一个简单的Annotations标记注解，表示是否将注解信息添加在java文档中。

#### @Retention : 定义该注解的生命周期。
		
		RetentionPolicy.SOURCE
		在编译阶段丢弃。
		
		RetentionPolicy.CLASS
		在类加载的时候丢弃。
		
		RetentionPolicy.RUNTIME
		 始终不会丢弃，运行期也保留该注解。
		
#### @Target : 表示该注解用于什么地方。如果不明确指出，该注解可以放在任何地方。需要说明的是：属性的注解是兼容的，如果你想给7个属性都添加注解，仅仅排除一个属性，那么你需要在定义target包含所有的属性。

``` java
@Retention(RetentionPolicy.SOURCE)
@Target({ElementType.FIELD,ElementType.TYPE,ElementType.METHOD})
public @interface TestTarget {

}
```

**ElementType.TYPE:用于描述类、接口或enum声明**
``` java
@TestTarget
public interface Annotations {
    
}

@TestTarget
public class Annotations {

}

@TestTarget
public enum  Annotations {

}

```

**ElementType.FIELD:用于描述实例变量**

``` java
public class Annotations {
    
    @TestTarget public String name;
    
}
```

**ElementType.METHOD:用于描述方法**

``` java
public class Annotations {
    
    @TestTarget
    public void test(){
        
    }

}

```

**ElementType.PARAMETER:用于描述参数**

``` java
public class Annotations {
    
    public void test(@TestTarget String name){

    }

}
```

**ElementType.CONSTRUCTOR:用于描述构造方法**

``` java
public class Annotations {

    @TestTarget
    public Annotations() {
    }
}

```

**ElementType.LOCAL_VARIABLE:用于描述局部变量**

``` java 
public class Annotations {

    public void test(){

        @TestTarget String name;

    }
}

```

**ElementType.ANNOTATION_TYPE:用于描述另一个注释**

``` java
public class Annotations {

    @TestTarget
    public @interface TestAnnotationType {

    }
}
```

**ElementType.PACKAGE 用于记录java文件的package信息**

``` java
@TestTarget
package com.helkay.common;

```
需要注意的一点是package注解只能写在package-info.java这个文件中


**since 1.8 加入**

**ElementType.TYPE_PARAMETER:用于描述类型参数**

``` java
public class Annotation<@TestTarget T> {

}
```


**ElementType.TYPE_USE:用于描述类型使用**
``` java
public class Annotation<@TestTarget T> {

    public List<@TestTarget T> test(@TestTarget String name){

        List<@TestTarget T> list1 = new ArrayList<>();

        List<? extends T> list2 = new ArrayList<@TestTarget T>();

        @TestTarget String text;

        text = (@TestTarget String)new Object();

        java.util. @TestTarget Scanner console;

        console = new java.util.@TestTarget Scanner(System.in);

        return new ArrayList<@TestTarget T>();
    }
}
```

#### @Inherited : 定义该注释和子类的关系

***Annotations只支持基本类型、String及枚举类型***

``` java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@interface TestTarget {

    public enum Sex {MAN, WOMAN}

    String author() default "Helkay";

    Sex sex () default Sex.MAN;
}
```

***如何使用:***

``` java
public class Annotation {

    @TestTarget( sex = TestTarget.Sex.WOMAN , author = "Hiko")
    public void test(){

    }
}
```

***只有一个属性***

``` java
@interface TestTarget{
String value();
}

@ TestTarget("Helkay")
public void test() {
}
```


#### @Native : 仅仅用来标记native的属性

#### @Repeatable : 可重复注解的注解

**不使用Repeatable**

``` java 
public class Annotation {

    @interface TestTarget {
        String name();
    }

    @interface TestTargetArr {
        TestTarget[] value();
    }

    @TestTargetArr({@TestTarget(name = "Helkay"),@TestTarget(name = "Hiko")})
    public void test(){

    }
}
```

**使用Repeatable**

``` java
public class Annotation {

    @Repeatable(TestTargetArr.class)
    @interface TestTarget {
        String name();
    }

    @interface TestTargetArr {
        TestTarget[] value();
    }

    @TestTarget(name = "Helkay")
    @TestTarget(name = "Hiko")
    public void test(){

    }
}
```

