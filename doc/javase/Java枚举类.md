### 枚举类

枚举（enum）类型是 `Java 5` 新增的特性，它是一种新的类型，允许用常量来表示特定的数据片断，而且全部都以类型安全的形式来表示。

### 枚举类定义

-  `enum` 和 `class` 、 `interface` 具有相同地位
- 默认继承 `java.lang.Enum` 类，所以不能继承其他类，其中 `java.lang.Enum` 类实现了 `java.lang.Serializable` 和 `java.lang.Comparable` 接口
- 使用 `enum` 定义，默认使用 `final` 修饰，因此不能派生子类
- 构造器默认使用 `private` 修饰，且只能使用 `private` 修饰
- 枚举类的所有实例必须在枚举类的第一行显式列出，否则这个枚举类永远不能产生实例
- 列出这些实例时，会自动添加 `public static final` 修饰，无需显式添加

#### 无构造函数的枚举类

```
/**
 * 枚举类
 */
public class Demo {
    public static void main(String[] args) {
        //声明一个枚举类实例，并赋值
        Season season = Season.SPRING;
        switch(season) {
            case SPRING:
                System.out.println("Spring");
                break;
            case SUMMER:
                System.out.println("Summer");
                break;
            case FALL:
                System.out.println("Fall");
                break;
            case WINTER:
                System.out.println("Winter");
                break;
            default:
                break;
        }
    }
}

/**
 * 定义一个枚举类
 */
enum Season{
    SPRING, SUMMER, FALL, WINTER
}
```

#### 有构造函数的枚举类

```
public enum Health{

    GOOD(0, "良好"),
    SICK(1, "生病"),
    FINE(2, "健康");

    private int code;
    private String name;

    private Health(int code, String name) {
        this.code = code;
        this.name = name;
    }

    public int getCode() {
        return code;
    }

    public String getName() {
        return name;
    }
}
```

### 枚举的方法

```
int compareTo(Enum e):用于比较两个枚举类型的顺序，如果该枚举类型位于枚举类型e之后，则返回正数，反之返回负数，相同返回零
values():返回一个枚举类型的数组，可以用来遍历枚举类
String name():返回枚举实例的枚举值。
int ordinal():返回枚举实例的声明顺序，从0开始
T valueOf(Class<T> enumType, String name):返回带指定名称的指定枚举类型的枚举常量。
boolean equals():用来比较两个枚举对象的枚举值。相同返回true，反之返回false
				  也可以使用 == 来比较
				  
public class Demo2 {
    public static void main(String[] args) {
        Health health = Health.GOOD;
        Health health1 = Health.FINE;
        Health health2 = Health.SICK;
        Health health3 = Health.GOOD;

        //compareTo()
        System.out.println(health.compareTo(health1));
        System.out.println(health1.compareTo(health2));
        //values()
        Health[] healths = Health.values();
        for(Health h : healths) {
            System.out.print(h + " ");
        }
        System.out.println();
        //name()
        System.out.println(health.name());
        //ordinal()
        System.out.println(health2.ordinal());
        //valueOf()
        Health health4 = Health.valueOf(Health.class, health2.name());
        System.out.println(health4);
        //equals()
        System.out.println(health.equals(health3));
        System.out.println(health == health3);
    }
}
/**
 * 枚举类
 */
enum Health{
    GOOD(0, "良好"),
    SICK(1, "生病"),
    FINE(2, "健康");

    private int code;
    private String name;

    private Health(int code, String name) {
        this.code = code;
        this.name = name;
    }

    public int getCode() {
        return code;
    }

    public String getName() {
        return name;
    }
}
```

### 枚举实现原理

实际上在使用关键字 `enum` 创建枚举类型并编译后，编译器会为我们生成一个相关的类，这个类继承了 `Java API` 中的 `java.lang.Enum` 类，也就是说通过关键字 `enum` 创建枚举类型在编译后事实上也是一个类类型而且该类继承自 `java.lang.Enum` 类

### 总结

枚举类可以代替常量来使用，比如：状态码，星期等，结合实际场景使用。

