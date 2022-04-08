## Java泛型

Java 泛型（generics）是 JDK 5 中引入的一个新特性, 泛型提供了编译时类型安全检测机制，该机制允许程序员在编译时检测到非法的类型。

泛型即参数化类型，也就是说数据类型变成了一个可变的参数，在不使用泛型的情况下，参数的数据类型都是写死了的，使用泛型之后，可以根据程序的需要进行改变。

### 为什么要使用泛型？

泛型程序设计（Generic programming) 意味着编写的代码可以被很多不同类型的对象所重用。

泛型正是我们需要的程序设计手段。使用泛型机制编写的程序代码要比那些杂乱地使用 `Object` 变量，然后再进行强制类型转换的代码具有更好的安全性和可读性。泛型对于集合类尤其有用，例如， `ArrayList` 就是一个无处不在的集合类。

**没有泛型前** 

```
private static void genericTest() {
    List arrayList = new ArrayList();
    arrayList.add("总有刁民想害朕");
    arrayList.add(7);

    for (int i = 0; i < arrayList.size(); i++) {
        Object item = arrayList.get(i);
        if (item instanceof String) {
            String str = (String) item;
            System.out.println("泛型测试 item = " + str);
        }else if (item instanceof Integer)
        {
            Integer inte = (Integer) item;
            System.out.println("泛型测试 item = " + inte);
        }
    }
}
```

如上代码所示，在没有泛型之前 **类型的检查** 和 **类型的强转** 都必须由我们程序员自己负责，一旦我们犯了错，就是一个运行时崩溃等着我们。

**有了泛型后** 

```
private static void genericTest2() {
     List<String> arrayList = new ArrayList<>();
     arrayList.add("总有刁民想害朕");
     arrayList.add(7); //..(参数不匹配：int 无法转换为String)
     ...
 }
```

如上代码，编译器在编译时期即可完成 **类型检查** 工作，并提出错误（其实IDE在代码编辑过程中已经报红了）

泛型提供了一个更好的解决方案： 类型参数（ type parameters)。ArrayList 类有一个类型参数用来指示元素的类型：

```
ArrayList<String> files = new ArrayList<String>():
```

这使得代码具有更好的可读性。人们一看就知道这个数组列表中包含的是 `String` 对象

参数化类型:

- 把类型当作是参数一样传递
- `<数据类型>` 只能是引用类型

- `ArrayList<E>` 中的 **E** 称为类型参数变量
- `ArrayList<Integer>` 中的 **Integer** 称为实际类型参数
- 整个称为 `ArrayList<E>` 泛型类型
- 整个 `ArrayList<Integer>` 称为参数化的类型 `ParameterizedType` 

```
注：在Java中，经常用T、E、K、V等形式的参数来表示泛型参数。

T：代表一般的任何类。
E：代表 Element 的意思。
K：代表 Key 的意思。
V：代表 Value 的意思，通常与 K 一起配合使用。
```

### 定义泛型的规则

- 只能是引用类型，不能是简单数据类型。
- 泛型参数可以有多个。
- 可以用使用 `extends` 语句或者 `super` 语句。如 `<T extends superClass>` 表示类型的上界，`T` 只能是 `superClass` 或其子类， `<K super childClass>` 表示类型的下界，`K` 只能是 `childClass` 或其父类。
- 可以是通配符类型，比如常见的 `Class<?>` 。单独使用 `?` 可以表示任意类型。也可以结合 `extends` 和 `super` 来进行限制。

### 使用泛型的好处

- 代码更加简洁【不用强制转换】
- 程序更加健壮【只要编译时期没有警告，那么运行时期就不会出现 `ClassCastException` 异常】
- 可读性和稳定性【在编写集合的时候，就限定了类型】

### 泛型类

**泛型类就是把泛型定义在类上，用户使用该类的时候，才把类型明确下来** ….这样的话，用户明确了什么类型，该类就代表着什么类型…用户在使用的时候就不用担心强转的问题，运行时转换异常的问题了。

```
/**
 * Java泛型
 */
public class Demo {
    public static void main(String[] args) {
        // 定义泛型类 Test 的一个Integer版本
        Test<Integer> intOb = new Test<Integer>(88);
        intOb.showType();
        int i = intOb.getOb();
        System.out.println("value= " + i);
        System.out.println("----------------------------------");
        // 定义泛型类Test的一个String版本
        Test<String> strOb = new Test<String>("Hello Gen!");
        strOb.showType();
        String s = strOb.getOb();
        System.out.println("value= " + s);
    }
}
/*
使用T代表类型，无论何时都没有比这更具体的类型来区分它。如果有多个类型参数，我们可能使用字母表中T的临近的字母，比如S。
*/
class Test<T> {
    private T ob;

    /*
    定义泛型成员变量，定义完类型参数后，可以在定义位置之后的方法的任意地方使用类型参数，就像使用普通的类型一样。
    注意，父类定义的类型参数不能被子类继承。
    */

    //构造函数
    public Test(T ob) {
        this.ob = ob;
    }

    //getter 方法
    public T getOb() {
        return ob;
    }


    //setter 方法
    public void setOb(T ob) {
        this.ob = ob;
    }

    public void showType() {
        System.out.println("T的实际类型是: " + ob.getClass().getName());
    }
}

/* output
    T的实际类型是: java.lang.Integer
    value= 88
    ----------------------------------
    T的实际类型是: java.lang.String
    value= Hello Gen!
*/
```

用户想要使用哪种类型，就在创建的时候指定类型。使用的时候，该类就会自动转换成用户想要使用的类型了

### 泛型接口

泛型接口与泛型类的定义基本一致

```
public interface Generator<T> {
    public T produce();
}
```

### 泛型方法

前面已经介绍了如何定义一个泛型类。实际上，还可以定义一个带有类型参数的简单方法。我们可能就仅仅在 **某一个方法上需要使用泛型** …. **外界仅仅是关心该方法，不关心类其他的属性** …这样的话，我们在整个类上定义泛型，未免就有些大题小作了。

你可以写一个泛型方法，该方法在调用时可以接收不同类型的参数。根据传递给泛型方法的参数类型，编译器适当地处理每一个方法调用。

下面是定义泛型方法的规则：

- 所有泛型方法声明都有一个类型参数声明部分（由尖括号分隔），该类型参数声明部分在方法返回类型之前（在下面例子中的 **<E>**）。
- 每一个类型参数声明部分包含一个或多个类型参数，参数间用逗号隔开。一个泛型参数，也被称为一个类型变量，是用于指定一个泛型类型名称的标识符。
- 类型参数能被用来声明返回值类型，并且能作为泛型方法得到的实际参数类型的占位符。
- 泛型方法体的声明和其他方法一样。注意类型参数只能代表引用型类型，不能是原始类型（像 **int、double、char** 等）。

**java中泛型标记符** 

- **E** - Element (在集合中使用，因为集合中存放的是元素)
- **T** - Type（Java 类）
- **K** - Key（键）
- **V** - Value（值）
- **N** - Number（数值类型）
- **？** - 表示不确定的 java 类型

```
public class Generic<T> {
    public T name;
    public  Generic(){}
    public Generic(T param){
        name=param;
    }
    public T m(){
        return name;
    }
    public <E> void m1(E e){ }
    public <T> T m2(T e){ }
}
```

重点看 `public  void m1(E e){ }` 这就是一个泛型方法，判断一个方法是否是泛型方法关键看方法返回值前面有没有使用 `<>` 标记的类型，有就是，没有就不是。这个 `<>` 里面的类型参数就相当于为这个方法声明了一个类型，这个类型可以在此方法的作用块内自由使用。 上面代码中， `m()` 方法不是泛型方法， `m1()` 与 `m2()` 都是。值得注意的是 `m2()` 方法中声明的类型 `T` 与类申明里面的那个参数 `T` 不是一个，也可以说方法中的 `T`  **隐藏** 了类型中的 `T` 。下面代码中类里面的 `T` 传入的是 `String` 类型，而方法中的 `T` 传入的是 `Integer` 类型。

```
Generic<String> str=new Generic<>("总有刁民想害朕");
str.m2(123);
```

```
/**
 * Java泛型 - 泛型方法
 */
public class Demo1 {

    // 定义泛型方法
    public static <T> void show(T t) {
        System.out.println(t);
    }

    public static void main(String[] args) {
        Demo1 demo1 = new Demo1();
        demo1.show("hello"); // 此时泛型为String类型
        demo1.show(22); // 此时泛型为Integer类型
    }
}
```

### 泛型类的继承规则

前面我们已经定义了泛型类， **泛型类是拥有泛型这个特性的类，它本质上还是一个Java类，那么它就可以被继承** 

- **如果传入具体参数，则子类不需要指定类型参数** 

```
/**
 * Java泛型 - 继承规则
 */
interface Generic<T> {
    void show (T t);
}

// 子类明确泛型类的类型参数
class Gener2 implements Generic<String> {

    @Override
    public void show(String s) {
        System.out.println(s);
    }
}

public class Demo2 {
    public static void main(String[] args) {
        Generic<String> generic1 = new Gener2();
        generic1.show("hello world");
    }
}
```

- **如果不传入具体的类型，则子类也需要指定类型参数** 

```
/**
 * Java泛型 - 继承规则
 */
interface Generic<T> {
    void show (T t);
}
// 子类不明确泛型类的类型参数
class Gener1<T> implements Generic<T> {

    @Override
    public void show(T t) {
        System.out.println(t);
    }
}
public class Demo2 {
    public static void main(String[] args) {
        Generic<Integer> generic = new Gener1<>();
        generic.show(2);

        Generic<String> generic1 = new Gener2();
        generic1.show("hello world");
    }
}
```

值得注意的是：

- 实现类要是重写父类的方法，返回值的类型是要和父类一样的！
- 类上声明的泛形只对非静态成员有效

### 类型通配符

**?号通配符，表示可以匹配任意类型，任意的Java类都可以匹配** …..

现在非常值得注意的是，当我们使用?号通配符的时候： **就只能调对象与类型无关的方法，不能调用对象与类型有关的方法。** 

```
public void test(List<?> list){

    for(int i=0;i<list.size();i++){
        System.out.println(list.get(i));
    }
}
```

记住， **只能调用与对象无关的方法，不能调用对象与类型有关的方法** 。因为直到外界使用才知道具体的类型是什么。也就是说，在上面的List集合，我是不能使用add()方法的。 **因为add()方法是把对象丢进集合中，而现在我是不知道对象的类型是什么。** 

#### 设定通配符上限

```
<? extends Type>  传递进来的只能是Type或者Type的子类

public void printIntValue(List<? extends Number> list) {  
    for (Number number : list) {  
        System.out.print(number.intValue()+" ");   
    }  
}

无论传入的是何种类型的集合，我们都可以使用其父类(Number)的方法统一处理
```

#### 设定通配符下限

```
<? super Type>  传递进来的只能是Type或者Type的父类

public void fillNumberList(List<? super Number> list) {  
    list.add(new Integer(0));  
    list.add(new Float(1.0));  
}
```

值得注意的是： **无论是设定通配符上限还是下限，都是不能操作与对象有关的方法，只要涉及到了通配符，它的类型都是不确定的！** 

### 泛型在静态中的问题

泛型类中的静态方法和静态变量不可以使用泛型类所声明的泛型类型参数，例如下面的代码编译失败

```
public class Test<T> {      
    public static T one;   //编译错误      
    public static  T show(T one){ //编译错误      
        return null;      
    }      
}
```

因为静态方法和静态变量属于类所有，而泛型类中的泛型参数的实例化是在创建泛型类型对象时指定的，所以如果不创建对象，根本无法确定参数类型。但是 **静态泛型方法** 是可以使用的，我们前面说过，泛型方法里面的那个类型和泛型类那个类型完全是两回事。

```
public static <T>T show(T one){   
     return null;      
 }
```

### 通配符和泛型方法

**大多时候，我们都可以使用泛型方法来代替通配符的** 

```
  //使用通配符
    public static void test(List<?> list) {

    }
    //使用泛型方法
    public <T> void  test2(List<T> t) {

    }
```

原则：

- 如果 **参数之间的类型有依赖关系** ，或者 **返回值是与参数之间有依赖关系** 的。那么就使用 **泛型方法** 
- 如果 **没有依赖关系** 的，就使用 **通配符** ，通配符会 **灵活一些.** 

### 泛型擦除

为什么人们会说 `Java` 的泛型是 **伪泛型** 呢，就是因为 `Java` 在编译时 **擦除** 了所有的泛型信息，所以 `Java` 根本不会产生新的类型到字节码或者机器码中，所有的泛型类型最终都将是一种 **原始类型** ，那样在 `Java` 运行时根本就获取不到泛型信息。

泛型是 **提供给javac编译器使用的** ，它用于限定集合的输入类型，让编译器在源代码级别上，即挡住向集合中插入非法数据。但编译器编译完带有泛形的java程序后， **生成的class文件中将不再带有泛形信息** ，以此使程序运行效率不受到影响，这个过程称之为“擦除”。

`Java` 编译器 **编译泛型** 的步骤

- **检查** 泛型的类型 ，获得目标类型
- **擦除** 类型变量，并替换为限定类型（T为无限定的类型变量，用Object替换）
- 调用相关函数，并将结果 **强制转换** 为目标类型。

```
public static void main(String[] args) {
        ArrayList<String> arrayString=new ArrayList<String>();
        ArrayList<Integer> arrayInteger=new ArrayList<Integer>();
        System.out.println(arrayString.getClass()==arrayInteger.getClass());
    }
/* output
	true
*/
```

上面代码输出结果为 **true** ，可见通过运行时获取的类信息是完全一致的，泛型类型被擦除了！

**擦除之前** 

```
//泛型类型  
class Pair<T> {    
    private T value;    
    public T getValue() {    
        return value;    
    }    
    public void setValue(T  value) {    
        this.value = value;    
    }    
}
```

**擦除之后** 

```
//原始类型  
class Pair {    
    private Object value;    
    public Object getValue() {    
        return value;    
    }    
    public void setValue(Object  value) {    
        this.value = value;    
    }    
}
```

因为在 `Pair<T>` 中，`T` 是一个无限定的类型变量，所以用 `Object` 替换。如果是 `Pair<T extends Number>`，擦除后，类型变量用 `Number` 类型替换。

### 泛型使用的限制

- Java泛型不能使用基本类型

```
List<int> list = new List<int>();// 编译前类型检查报错
```

- Java泛型不允许进行直接实例化

```
<T> void test(T t){
    t = new T();//编译前类型检查报错
}
```

- Java泛型不允许进行静态化

```
class StaticGeneric<T>{
        private static T t;// 编译前类型检查报错

        public static T getT() {// 编译前类型检查报错
            return t;
        }
    }
静态变量在类中共享，而泛型类型是不确定的，因此编译器无法确定要使用的类型，所以不允许进行静态化
```

- Java泛型不允许直接进行类型转化（通配符可以）

```
List<Integer> integerList = new ArrayList<Integer>();
List<Double> doubleList = new ArrayList<Double>();
//不能直接进行类型转换，类型检查报错
integerList = doubleList;

static void cast(List<?> orgin, List<?> dest){
    dest = orgin;
}
```

- Java泛型不允许直接使用instanceof运算符进行运行时类型检查（通配符可以）

```
List<String> stringList = new ArrayList<String >();
//不能直接使用instanceof，类型检查报错
System.out.println(stringList instanceof ArrayList<Double>);
```

- Java泛型不允许创建确切类型的泛型数组（通配符可以）

```
//类型检查错误
List<Integer>[] list = new ArrayList<Integer>[2];
//通配符创建
Generic<?>[] generics = new Generic<?>[2];
generics[0] = new Generic<Integer>(123);
generics[1] = new Generic<String>("hello");
for (Generic<?> generic : generics) {
    System.out.println(generic.get());
}
```

- Java泛型不允许作为参数进行重载

```
public class GenericTest<T>{
    void test(List<Integer> list){}
    //不允许作为参数列表进行重载
    void test(List<Double> list){}
}
类型擦除后两个方法是一样的参数列表，无法重载。
```

### 总结

本文中的例子主要是为了阐述泛型中的一些思想而简单举出的，并不一定有着实际的可用性。另外，一提到泛型，相信大家用到最多的就是在集合中，其实，在实际的编程过程中，自己可以使用泛型去简化开发，且能很好的保证代码质量。
