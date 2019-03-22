# Java程序创建对象的方式

## 1、使用new关键字创建

使用new关键字调用对象的的构造方法。

```java
//调用无参构造方法
Person person = new Person();
//调用有参构造方法
String name = "testName";
Person person = new Person(name);
```

## 2、使用newInstance()方法创建

利用java.lang.Class类的newInstance方法，则可根据Class对象的实例，建立该Class所表示的类的对象实例。

```java
        try {
            Person personInstace = Person.class.newInstance();
        } catch (InstantiationException e) {//如果这个类是无法创建实例的类
            e.printStackTrace();
        } catch (IllegalAccessException e) {//其类为空或者无参构造方法不可访问
            e.printStackTrace();
        }
//或者使用类路径
        String path = "test.Student";
        try {
            Class claz = Class.forName(path);
            Student student = (Student) claz.newInstance();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (InstantiationException e) {
            e.printStackTrace();
        }
```

**注意**：使用newInstance()方法创建对象实例，所创建对象类中必须有无参构造方法

## 3、用克隆创建

浅克隆方式，实现Cloneable接口，重写Object类中的clone()方法。

Object中的clone方法：

```java
protected native Object clone() throws CloneNotSupportedException;
```

Object中clone方法是protected。

1. 基类的 protected 成员是包内可见的，并且对子类可见；
2. 若子类与基类不在同一包中，那么在子类中，子类实例可以访问其从基类继承而来的protected方法，而不能访问基类实例的protected方法。

克隆创建代码：

```java

public class Test {
    public static void main(String[] args) {
        Person person1 = new Person();
        Person person2 = person1.clone();
        System.out.println(person1==person2);//false
    }
}

 class Person implements Cloneable {
    Person() {
        System.out.println("这个是Person类！");
    }
  
    @Override
    public Person clone(){
        Person person = null;
        try {
            person = (Person) super.clone();
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
        return person;
    }
}
```

## 4、使用反序列化

当我们序列化和反序列化一个对象，jvm会给我们创建一个单独的对象。在反序列化时，jvm创建对象并不会调用任何构造函数。为了反序列化一个对象，我们需要让我们的类实现Serializable接口