## Java接口

### 什么是接口呢？ 

在 `Java` 中，接口是一种抽象类型，是抽象方法的集合。接口使用 `interface` 来声明。

```
public interface Demo {
	public abstract show();
}
```

### 接口的特性

- 接口中包含全局常量，抽象方法

  全局常量 - 接口会默认的为全局常量设置为 `public static final` 

  抽象方法 - 接口会默认的为抽象方法设置为 `public abstract` 

- 接口中的方法为抽象方法，因此接口不能被实例化

  类可以实现接口并实例化对象，接口可以声明接口变量来引用该类的对象（ `Java` 多态）

- 类与类之间是继承关系，类与接口之间是实现关系，使用关键字 `implements` 

- 类只能继承一个类，但是一个类可以实现多个接口（解决了 `Java` 单继承）

- 可以使用 `instanceof` 来检查一个类是否实现某个特定的接口

- `Java` 是单继承（一个类只能继承一个类）接口是多继承（一个接口可以继承多个接口）

```
/**
 * 定义一个接口
 */
public interface Demo {
    default void show() {
        //操作
    }
}

interface King {
    // 全局变量 ，默认提供 public static final
    double PI = 3.14;
    // 抽象方法 ，默认提供 public abstract
    void show();
}

//接口多继承，声明超接口的方法
interface Dom extends Demo, King{

    void show();
}

//类实现接口
class Student implements King{

    // 类实现接口，就需要实现接口中的抽象方法
    @Override
    public void show() {
        System.out.println(PI);
    }
}

class Test {
    public static void main(String[] args) {
        // 接口对象 来引用 类对象  - 体现了面向对象的多态
        King king = new Student();
        king.show();

        // 判断类是否实现了接口
        Student student = new Student();
        System.out.println(student instanceof King);
    }
}
```

### 接口和抽象类的区别

1. 抽象类只能被继承，而且只能单继承

   接口需要被实现，而且可以多实现

2. 抽象类中可以定义非抽象方法，子类可以直接继承使用

   接口中都有抽象方法，需要子类实现

3. 抽象类使用的是 `is a` 关系

   接口使用的是 `like a` 关系

4. 抽象类的成员修饰符可以自定义

   接口中的成员修饰符是固定的，全部是 `public` 的

5. 抽象类有构造方法

   接口没有构造方法

6. 抽象类中可以有静态代码块和静态方法

   接口中不能含有静态代码块和静态方法

### 接口的默认方法

可以为接口提供一个默认实现，必须使用 `default` 修饰符来标记

```
public interface Demo {
    default void show() {
        //操作
    }
}
```

### 接口的设计特点

1. 接口是对外提供的规则
2. 接口是功能上的扩展
3. 接口的出现降低了耦合性

### 接口的应用

```
/**
 * 声明接口USB
 */
interface USB {
    void used();
}

/**
 * 声明类Mouse，实现USB接口，并实现used方法
 */
class Mouse implements USB {

    @Override
    public void used() {
        System.out.println("鼠标点击");
    }
}

/**
 * 声明类KeyBoard，实现USB接口，并实现used方法
 */
class KeyBoard implements USB {

    @Override
    public void used() {
        System.out.println("键盘按键");
    }
}

public class Demo1 {
    public static void main(String[] args) {
        //声明接口对象，引用鼠标对象
        USB usb = new Mouse();
        usb.used();
        //声明接口对象，引用键盘对象
        usb = new KeyBoard();
        usb.used();
    }
}

```
