## Java内部类

将一个类的定义放在另一个类的定义内部，这就是内部类。而包含内部类的类被称为外部类

### 内部类分类

- 成员内部类
- 局部内部类
- 匿名内部类
- 静态内部类

### 成员内部类

成员内部类是最普通的一种内部类，成员内部类可以访问外部类所有的属性和方法。但是外部类要访问成员内部类的属性和方法，必须要先实例化成员内部类。

```
public class Demo {

    private String name = "Tom";

    // 定义一个成员内部类，作为成员变量
    class Student{
        private int age = 20;
        private String name = "Jack";

        public void show() {
            System.out.println("内部类属性；" + name);
            System.out.println("内部类属性：" + age);
            System.out.println("外部类属性：" + Demo.this.name);
        }
    }

    public static void main(String[] args) {
        //声明一个外部类对象
        Demo demo = new Demo();
        //声明一个内部类对象
        Student student = demo.new Student();
        student.show();
    }
}
```

#### 成员内部类的使用方法

- 内部类相当于外部类的一个成员变量，所以内部类可以使用任意修饰符
- 内部类在外部类里面，所以内部类里可以直接访问外部类的方法和属性，反之不行
- 定义成员内部类后，必须使用外部类对象来创建内部类对象，即 `内部类 对象名 = 外部类对象.new 内部类();` 
- 如果外部类和内部类具有相同的成员变量或方法，内部类默认访问自己的成员变量或方法，如果要访问外部类的成员变量，可以使用 `this` 关键字。

注：成员内部类不能含有 `static` 的变量和方法，因为成员内部类需要先创建了外部类，才能创建它自己的

### 静态内部类

静态内部类通常被称为嵌套类。

静态内部类是不需要依赖于外部类的，这点和类的静态成员属性有点类似，并且它不能使用外部类的非static成员变量或者方法，这点很好理解，因为在没有外部类的对象的情况下，可以创建静态内部类的对象，如果允许访问外部类的非static成员就会产生矛盾，因为外部类的非static成员必须依附于具体的对象

```
public class Demo1 {
    private String name = "static";

    private static int id = 2019;

    static class Student{
        int id = 11;

        public void show() {
            System.out.println("外部类成员变量:" + (new Demo1().name));
            System.out.println("外部类静态成员:" + Demo1.id);
            System.out.println("内部类成员变量:" + id);
        }

    }

    public static void main(String[] args) {
        Student student = new Student();
        student.show();
    }
}
```

#### 静态内部类特点

- 静态内部类不能直接访问外部类的非静态成员，但可以通过 `new 外部类().成员` 的方式访问
- 如果外部类的静态成员与内部类的成员名称相同，可通过 `类名.静态成员` 访问外部类的静态成员；如果外部类的静态成员与内部类的成员名称不相同，则可通过 `成员名` 直接调用外部类的静态成员
- 创建静态内部类的对象时，不需要外部类的对象，可以直接创建 `内部类 对象名 = new 内部类();` 

### 局部内部类

局部内部类，是指内部类定义在方法和作用域内

局部内部类也像别的类一样进行编译，但只是作用域不同而已，只在该方法或条件的作用域内才能使用，退出这些作用域后无法引用的。

```
/**
 * 局部内部类
 */
public class Demo2 {

    //将内部类定义在方法中
    public void show() {
        final String name = "Tom";
        class Student{
            int id = 20;
            public void show() {
                System.out.println("外部类方法变量:" + name);
                System.out.println("内部类属性变量:" + id);
            }
        }
        Student student = new Student();
        student.show();
    }

    //将内部类定义在作用域内
    public void display(int num) {
        if(num > 2) {
            class Person{
                int age = 18;

                public void show() {
                    System.out.println("age = " + age);
                }
            }
            Person person = new Person();
            person.show();
        } else {
            System.out.println("None");
        }
    }

    public static void main(String[] args) {
        Demo2 demo2 = new Demo2();
        demo2.show();
        demo2.display(3);
    }
}
```

#### 局部内部类特点

- 局部内部类就像是方法里面的一个局部变量一样，是不能有 `public` 、 `protected` 、 `private`  以及 `static` 修饰符的。
- 局部内部类可以被 `final` 修饰。

### 匿名内部类

匿名内部类，顾名思义，就是没有名字的内部类。正因为没有名字，所以匿名内部类只能使用一次，它通常用来简化代码编写。但使用匿名内部类还有个前提条件：必须继承一个父类或实现一个接口

```
/**
 * 匿名内部类
 */
public class Demo3 {

    public static void main(String[] args) {
        Demo3 demo3 = new Demo3();
        // 匿名内部类 -> 类继承
        new Animal() {
          public void say() {
              System.out.println("Say");
          }
        }.say();
        // 匿名内部类 -> 接口实现
        new Fruit(){
            public void eat() {
                System.out.println("apple");
            }
        }.eat();
    }

}

class Animal{
    public void say() {
        System.out.println("Animal say");
    }
}

interface Fruit{
    void eat();
}
```

#### 匿名内部类特点

- 匿名内部类是 **不能加访问修饰符** 的。要注意的是， `new` 匿名类，这个类是要先定义的, 如果不先定义，编译时会报错该类找不到。
- 匿名内部类没有构造函数。

### 内部类的主要作用

- 内部类提供了更好的封装，可以把内部类隐藏在外部类之内，不允许同一个包中的其他类访问该类
- 内部类的方法可以直接访问外部类的所有数据，包括私有的数据
- 内部类所实现的功能使用外部类同样可以实现，只是有时使用内部类更方便
- 内部类允许继承多个非接口类型
