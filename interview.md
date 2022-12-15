#### Java基础

##### 包装类型的缓存机制

​		Byte、Short、Integer、Long这四种包装类默认创建了数值[-128,127]范围内相应类型的缓存数据，Character创建了[0,127]范围内的缓存数据，Float和Double没有缓存机制。总之，用equals方法比较相等。

```java
Integer i1 = 128;
Integer i2 = 128;
Integer i3 = 28;
Integer i4 = 28;
Integer i5 = new Integer(28);
System.out.println(i1 == i2);//false
System.out.println(i3 == i4);//true
System.out.println(i3 == i5);//false，不是new出来的都在常量池中，new出来的在堆中
```

##### 自动类型提升

① 若参与运算的数据类型不同，则先转换为同一类型，转换按数据长度增加的方向进行，以保证精度不降低；

② 浮点运算都是以双精度进行的，float会被转为double；

③ byte、short、char在做运算的时候会被提升为int，即使两个byte型的相加，结果类型也是int；

④ 赋值运算中，赋值符号两边数据类型不同时，需要将符号右边的类型强转，若右边的数据类型长度比左边大，将丢失一部分数据。

（byte、short、char）—> int —> long —> double <— float

##### switch支持long型数据吗

​		switch支持char, byte, short, int, Character, Byte, Short, Integer, String或枚举类，但本质都是转为int来进行判断，而long/Long型变量表示范围大于int/Integer，因此switch不支持long型数据。

##### 访问修饰符权限

private：仅限本类，封装的体现；

default：本包下；

protected：本包下及不同包的子类；

public：任意。

##### 重写与重载

​		重写是子类对父类、或继承接口的类对接口同名方法的override，遵循两同两小一大原则，即方法名相同、参数类型相同、子类返回类型小于等于父类返回类型、子类抛出的异常小于等于父类抛出异常、子类方法访问权限大于等于父类方法访问权限。

​		注意：不能重写父类的private方法和static方法，static方法是编译时静态绑定的，而方法重写是基于运行时动态绑定的；private修饰的方法只能在当前类使用，其他类访问不到该方法，自然也无法重写。

​		重载是允许同一个类中存在同名方法，只要方法参数个数、参数类型不同即可。

##### 静态块、普通块、构造器的执行顺序

​		这里的普通块是用{}扩起来的，实际上应该叫构造块。执行顺序为：父类静态块 —> 子类静态块 —> 父类普通块 —> 父类构造器 —> 子类普通块 —> 子类构造器。

##### 抽象类和接口的区别

① 语法层面上：

​	1）抽象类可以提供成员方法的实现细节，而接口中只能存在public abstract 方法；

​	2）抽象类中的成员变量可以是各种类型的，而接口中的成员变量只能是public static final类型的；

​	3）接口中不能含有静态代码块以及静态方法（JDK8允许默认方法，JDK9允许私有方法），而抽象类可以有静态代码块和静态方法；

​	4）一个类只能继承一个抽象类，而一个类却可以实现多个接口。

② 设计层面上：

​		抽象类是对一种事物的抽象，即对类抽象，而接口是对行为的抽象。抽象类是对整个类整体进行抽象，包括属性、行为，但是接口却是对类局部（行为）进行抽象。继承是一个”是不是“的关系，而接口实现则是 "能不能"的关系。

##### 深拷贝与浅拷贝

引用拷贝：仅仅拷贝该对象的引用，不新建对象；

浅拷贝：拷贝时会在堆上创建新对象，但如果该对象内部有引用类型的属性，只拷贝引用类型属性的地址；

深拷贝：完全复制整个对象，包括其内部包含的对象。

##### Java的值传递

​		程序设计语言将实参传递给方法的方法有值传递和引用传递。值传递是将实参的拷贝传到方法形参中，而引用传递是直接将实参的引用赋给形参，不创建副本，对形参的修改将影响到实参。

对于基本数据类型：

```java
public static void main(String[] args) {
    int number1 = 10;
    int number2 = 20;
    swap(number1, number2);
    System.out.println("number1 = " + number1);
    System.out.println("number2 = " + number2);
}

public static void swap(int number1, int number2) {
    int temp = number1;
    number1 = number2;
    number2 = temp;
    System.out.println("number1 = " + number1);
    System.out.println("number2 = " + number2);
}
```

输出：

```
number1 = 20
number2 = 10
------------
number1 = 10
number2 = 20
```

对于引用类型：

```java
public class Person {
    String name;

    public static void main(String[] args) {
        Person p1 = new Person("张");
        Person p2 = new Person("李");
        swap(p1,p2);
        System.out.println("person1:" + p1.name);
        System.out.println("person2:" + p2.name);
    }

    public static void swap(Person p1, Person p2) {
        Person temp = p1;
        p1 = p2;
        p2 = temp;
        System.out.println("person1:" + p1.name);
        System.out.println("person2:" + p2.name);
    }
}
```

输出：

```
person1:李
person2:张
---------
person1:张
person2:李
```

##### String

①String、StringBuffer、StringBuilder：String是不可变的，因为其底层数组被定义为private final，且没有暴露对字符串的修改方法，同时final修饰后不能被继承，避免了子类破坏String的不可变性。可以理解String为常量，线程安全。修改String需要新生成String再进行修改，而StringBuffer和StringBuilder都是在对象本身上操作，不同的是StringBuffer对方法加了同步锁，线程安全。通过“ + ”对字符串进行拼接，本质上还是调用了StringBuilder的append方法。

②String s1 = new String( "abc" )会创建几个对象？如果字符串常量池中没有“abc"的引用，那么会在堆中new一个对象，并将对该对象的引用保存至字符串常量池中，即创建了两个对象；若字符串常量池中已经存在了”abc"的引用，则只会创建1个对象。

```java
String s1 = new String("ab");
String s2 = new String("ab");
System.out.println(s1 == s2);//false

String s1 = "ab";
String s2 = new String("ab");
System.out.println(s1 == s2);//false

String s1 = "ab";
String s2 = "ab";
System.out.println(s1 == s2);//true
```

③intern方法：将指定字符串对象的引用保存到字符串常量池中，如果字符串常量池中已经有该字符串对象，就直接返回，反之就将该字符串加到常量池中并返回它在常量池中的引用。

```java
String s1 = "abc";
String s2 = s1.intern();
String s3 = new String("abc");
String s4 = s3.intern();
System.out.println(s1 == s2);//true，都是引用常量池中的
System.out.println(s1 == s4);//true，同理
System.out.println(s3 == s4);//false，s3引用的是常量池外的
```

④String类型的变量和常量的 “+” 操作：像String s3 = "str" + "ing"这种常量拼接，编译器会优化为String s3 = "string"，这称为常量折叠。而变量的加号拼接本质是调用了StringBuilder的append方法创建新对象。

```java
String s1 = "str";
String s2 = "ing";
String s3 = "str" + "ing";
String s4 = s1 + s2;
String s5 = "string";
System.out.println(s3 == s4);//false
System.out.println(s3 == s5);//true
System.out.println(s4 == s5);//false
```

##### 常量池

① 全局字符串常量池（String Pool）

​		字符串常量池的内容是在类加载完成，经过验证、准备阶段后在堆中生成字符串对象实例，然后将该字符串对象实例的引用值存到字符串常量池的StringTable中。对于字面量型的字符串String s1 = "abc"，是直接放在字符串常量池的非StringTable区域。也就是说，字符串常量池既存了引用值也存字面量。

② class文件常量池（class constant pool）

​		class文件中除了包含类的版本、字段、方法、接口等描述信息外，还有一项信息就是常量池，用于存放编译器生成的各种字面量(Literal)和符号引用(Symbolic References)。

③ 运行时常量池

​		java文件被编译成class文件后，就会生成class文件常量池。而当类加载到内存中后，JVM就会将class文件常量池中的内容放到运行时常量池中，将符号引用解析为直接引用。

##### 重写equals也要重写hashCode

​		Object类的hashCode方法是根据对象的存储地址转换形成一个哈希值，这时可能出现两个存储在不同位置的、但其实是相同的对象得出不同的哈希值，但规范其实是要求相同对象的哈希值一定要相同的。

##### 受检异常和非受检异常

​		受检异常在编译过程中，如果没有被catch或者throws，无法通过编译，如FileNotFoundException；而非受检异常是不处理也可以正常通过编译，如10/0这种ArithmeticException。

##### finally块中可以使用return吗

​		最好不要，try块中的return值会暂存到一个本地变量中，执行了finally的return后，finally返回的值会覆盖先前try中返回的值。

##### serialVersionID

​		在序列化时指明了serialVersionID且顺利完成了序列化，然后将该类的serialVersionID修改，会发现在反序列化时会报InvalidClassException异常；

​		如果没有指定serialVersionID就进行序列化，JVM其实也会根据类的属性自动生成一个，如果这时候修改了类的属性，比如增加一个属性，那在反序列化的时候，JVM根据属性生成的serialVersionID就不一样了，同样会报InvalidClassException异常。

​		注意：静态属性不会被序列化，因为序列化是针对对象而言的，而静态变量是随着类的加载而加载。serialVersionID其实也是static修饰的，所以它没被序列化。

##### BIO、NIO与AIO



#### Redis

##### 内存淘汰策略

① volatile-lru：使用近似LRU算法移除，仅适用于设置了过期时间的key；

② allkeys-lru：使用近似LRU算法移除，适用于所有类型的key；

③ volatile-lfu：使用近似LFU算法移除，仅适用于设置了过期时间的key；

④ allkeys-lfu：使用近似LFU算法移除，适用于所有类型的key；

⑤ volatile-random：随机移除key，仅适用于设置了过期时间的key；

⑥ allkeys-random：随机移除key，适用于所有类型的key；

⑦ noeviction：不移除任何内容，再进行写操作时直接返回error，是默认策略。

