---
title: 关于String的几个问题
tags:
  - String
  - 源码
top_img: false
categories:
  - 总结
date: 2021-12-15 00:31:30
abbrlink: String
---

### String str ="" 和 String str = new String("") 的区别

```java
String str1 = "str";
String str2 = "str";
String str3 = new String("str");
String str4 = new String("str");
System.out.println(str1==str2);//true
System.out.println(str3==str4);//false
```

`区别`

- 直接定义的`String str =""`是储存在常量存储区中的`字符串常量池`中；`String str = new String("")是储存在`堆`中
- 常量池中相同的字符串只会有一个，但是new String（）每new一个对象就会在堆中新建一个对象，而不管这个值是否相同
- str1和str3都指向字符串常量池中的“str”，所以str1==str2为true；

![image-20211214222221445](https://cdn.jsdelivr.net/gh/pinknee/img-bed/img/post/String/image-20211214222221445.png)

- `String str =""`在编译阶段就会在内存中创建；`String str = new String("")`是在运行时才会在堆中创建对象

`举个栗子对比一下就清晰了`

```java
String s1 = "nihao"; // 放在常量池中，没找到，新建一个
String s2 = "nihao"; // 从常量池中查找，找到了，直接引用。s1，s2指向同一个对象
String s3 = new String("nihao"); // s3 为一个引用
String s4 = new String("nihao"); // s4 也是一个引用。虽然s3，s4对象的内容一样，但它们却不是一个对象。
String s5 = "ni" + "bao"; //字符串常量相加，在编译时就会计算结果，s1 == s5 返回ture
String s6 = "ni"; 
String s7 = "hao"
String s8 = s6 + s7; //字符串变量相加，编译时无法计算，s1 == s8 返回false
class Student{
    String name;
    Person(String name) { 
        this.name = name;
    }
}
Person p1 = new Person("nihao");
Person p2 = new Person("nihao");
boolean b = p1.name == p2.name;//返回true
```

> - 这里要说明一下  字符串变量在相加时，会在堆中先开辟空间，再拼接，本质是new了StringBuilder对象进行了append操作，凭借之后toString()返回String对象
> - 而常量相加就简单了，先拼接，再在常量池中找，如果有直接返回地址，没有就创建。
> - 部分源码如下：



```java
@Override
public StringBuilder append(String str) {
    super.append(str);
    return this;
}
```

```java
@Override
public String toString() {
    // Create a copy, don't share the array
    return new String(value, 0, count);
}
```

### ==与equals

`==`

- 对于基本数据类型来说，==是比较值
- 对于引用数据类型来说，==是比较对象的引用

`equals`

 - Object.equals()

> Object.equals()使用的算法区分度高，只要两对象不是同一个就是错误的。由于所有的类都继承自Object类，所以equals()适用于所有对象。Object中的equals方法返回 == 的判断，即对象的地址判断。

```java
public boolean equals(Object obj) {
        return (this == obj);
    }
```

 - String.equals()

> 比较二者同为String类型，长度相等，且字符串值完全相同，包括顺序和值，不再要求两者为同一对象

```java
public boolean equals(Object anObject) {
        if (this == anObject) {
            return true;
        }
        if (anObject instanceof String) {
            String anotherString = (String)anObject;
            int n = value.length;
            if (n == anotherString.value.length) {
                char v1[] = value;
                char v2[] = anotherString.value;
                int i = 0;
                while (n-- != 0) {
                    if (v1[i] != v2[i])
                        return false;
                    i++;
                }
                return true;
            }
        }
        return false;
    }
```

`举个栗子`

```java
String str1 = "str";
String str2 = new String("str");
Object obj1 = new Object();
Object obj2 = new Object();
System.out.println(str1.equals(str2));//true
System.out.println(obj1.equals(obj2));//false
```



> 1.首先比较字符串str1和str2的引用是否相等，相等则返回（引用相同，则堆中的对象实例必然也相同）；
> 2.如果引用不同，那么比较String中的内容是否相同，如果相同则返回true.
> 3.String 的本质就是字符数据，看源码即可得知。

### 为什么要在继承Object类的类里面重写equals方法？

- String 是 java中的一个重要变量，如果每一个程序运行 ，jvm都给String new出来一个堆内存，无疑是浪费。
- 如果避免这种浪费，就用到了jvm方法区中的常量池。专门来存储字符串的内容。
- 每一个new 一个对象或者 申明一个时，JVM都会去方法区的常量池中去找是否包含“hello world”，这个字符串，如果有那么返回相同的引用地址。
- 反向思维：如果String不重写equals()方法，而是调用父类Object的equals方法。那么我们只是知道两个引用不同，里面内容无法得知是否相等。
- 然而在常用的开发中，我们对于String更加关系的是两个字符串中的内容是否相同，而不是是不是同一个引用。所以结合常量池，String有理由重写equals方法。

> 不单单是String，常用的Interger, Date等这些类都重写了equals()方法