# 注释

## 单行注释

```java
// 这是一个单行注释
```

## 多行注释

```java
/*
  这是一个多行注释
 */
```

## 文档注释

```java
/**
 * 这是一个文档注释
 */
```

**注意**: 文档注释一般是写在方法和类的上面.



---





# 基本数据类型

| 数据类型 (Data Type) | 字节 (Bytes) | 位数 (Bits)  | 默认值     | 取值范围 / 说明               |
| -------------------- | ------------ | ------------ | ---------- | ----------------------------- |
| **byte**             | 1            | 8            | 0          | `-128` 到 `127`               |
| **short**            | 2            | 16           | 0          | `-32768` 到 `32767`           |
| **int**              | 4            | 32           | 0          | `-2^31` 到 `2^31 - 1`         |
| **long**             | 8            | 64           | 0L         | `-2^63` 到 `2^63 - 1`         |
| **float**            | 4            | 32           | 0.0f       | 单精度浮点数                  |
| **double**           | 8            | 64           | 0.0d       | 双精度浮点数                  |
| **char**             | 2            | 16           | `'\u0000'` | 存储单个 Unicode 字符         |
| **boolean**          | **不确定**   | **逻辑上 1** | `false`    | 只有 `true` 和 `false` 两个值 |





---



# 关键字与标识符

## 关键字



关键字是预先定义好的赋予了特殊含义的单词.



*主要特点：*

- 全部小写.
- 被 Java 语言保留,不能用作标识符（即不能用它们来给变量、方法、类等命名）.
- 在常见的代码编辑器中,关键字通常会以高亮颜色显示.



## 标识符

标识符就是程序员在编程时为**变量、方法、类、接口、包**等程序元素自定义的名称。



*主要特点：*

- 可以由字母（A-Z, a-z）、美元符号（`$`）、下划线（`_`）和数字（0-9）组成.
- 不能以数字开头.
- 不能是 Java 的关键字或保留字（如 `int`, `class`, `goto` 等）.
- 严格区分大小写.



## 命名规范



这是业界的最佳实践,虽然不遵守也能编译通过,但强烈建议遵循.

1. **类名和接口名**：
   - 采用**大驼峰命名法 (Upper Camel Case)**.
   - 每个单词的首字母都大写.
   - 示例：`public class HelloWorld`, `interface Runnable`
2. **变量名和方法名**：
   - 采用**小驼峰命名法 (lower Camel Case)**.
   - 第一个单词首字母小写,从第二个单词开始,每个单词的首字母大写.
   - 示例：`int studentAge;`, `String userName;`, `void calculateSum()`
3. **常量名**：
   - 所有字母都**大写**，单词之间用**下划线（`_`）**连接.
   - 通常由 `public static final` 修饰.
   - 示例：`public static final int MAX_USERS = 100;`, `final double PI = 3.14;`
4. **包名**：
   - 所有字母都**小写**,通常是公司或组织的域名的反向形式.
   - 示例：`package com.google.common;`, `package java.util;`



---





# 方法重载



方法重载是面向对象的一种概念,方法重载就是指在同一个类里,允许多个方法拥有相同的名字,但它们的参数列表必须不同(类型不同,个数不同,顺序不同).



```java
class Printer {
    // 方法名叫 print
    public void print(String text) {
        System.out.println("打印文本: " + text);
    }

    // 方法名也叫 print，但参数类型不同 (int)
    public void print(int number) {
        System.out.println("打印数字: " + number);
    }

    // 方法名也叫 print，但参数数量不同 (两个)
    public void print(String text, int copies) {
        System.out.println("打印文本 '" + text + "'，共 " + copies + " 份");
    }
}

// 如何使用：
Printer myPrinter = new Printer();
myPrinter.print("Hello World"); // 调用第一个 print 方法
myPrinter.print(1024);         // 调用第二个 print 方法
myPrinter.print("报告", 3);      // 调用第三个 print 方法
```





---





# 逻辑运算符



| 运算符 | 名称                      | 运算类型          | 功能说明                                                     | 特点                                    |
| ------ | ------------------------- | ----------------- | ------------------------------------------------------------ | --------------------------------------- |
| `&`    | 按位与 / 逻辑与（非短路） | 位运算 / 布尔运算 | **位运算**：两个二进制位都为 `1` 才为 `1`；**布尔运算**：两个条件都为 `true` 才为 `true` | 不会短路，两个表达式都会执行            |
| `|`    | 按位或 / 逻辑或（非短路） | 位运算 / 布尔运算 | **位运算**：只要有一个二进制位为 `1` 就为 `1`；**布尔运算**：只要有一个条件为 `true` 就为 `true` | 不会短路，两个表达式都会执行            |
| `!`    | 逻辑非                    | 布尔运算          | 取反运算，`true → false`，`false → true`                     | 只能用于布尔类型                        |
| `^`    | 按位异或 / 逻辑异或       | 位运算 / 布尔运算 | **位运算**：两个二进制位不同为 `1`；**布尔运算**：两个条件不同为 `true` | 常用于判断差异或变量交换                |
| `&&`   | 逻辑与（短路）            | 布尔运算          | 两个条件都为 `true`，结果才为 `true`                         | **短路**：左边为 `false` 时，右边不执行 |
| `||`   | 逻辑或（短路）            | 布尔运算          | 只要有一个条件为 `true`，结果就为 `true`                     | **短路**：左边为 `ftrue` 时，右边不执行 |







---





# 后缀补全



## **Java 后缀补全用法大全**





这份表格适用于在 `.java` 文件中进行 Android 或其他 Java 项目开发.

|       分类       |        模板        |                    说明                     |            输入示例            |                           生成代码                           |
| :--------------: | :----------------: | :-----------------------------------------: | :----------------------------: | :----------------------------------------------------------: |
|   **变量声明**   |       `.var`       |       为表达式的结果创建一个局部变量        |  `new User("admin")`**.var**   |               `User user = new User("admin");`               |
|                  |      `.field`      | 为表达式的结果创建一个类的成员变量（Field） |  `new Button(this)`**.field**  |     `private final Button button;` <br/>(在类的顶部声明)     |
|     **循环**     |       `.for`       |  遍历一个 `Iterable` 对象（增强 for 循环）  |      `getUsers()`**.for**      |            `for (User user : getUsers()) { ... }`            |
|                  |      `.fori`       | 使用索引遍历数组或 `List`（传统 for 循环）  |      `userList`**.fori**       |     `for (int i = 0; i < userList.size(); i++) { ... }`      |
|                  |      `.forr`       |      使用索引**反向**遍历数组或 `List`      |      `userList`**.forr**       |   `for (int i = userList.size() - 1; i >= 0; i--) { ... }`   |
|  **条件与分支**  |       `.if`        |        基于布尔表达式生成 `if` 语句         |    `user.isActive()`**.if**    |                `if (user.isActive()) { ... }`                |
|                  |      `.else`       |    基于布尔表达式生成 `if (!expr)` 语句     |   `list.isEmpty()`**.else**    |                `if (!list.isEmpty()) { ... }`                |
|                  |     `.switch`      |         为表达式生成 `switch` 语句          |  `user.getRole()`**.switch**   |              `switch (user.getRole()) { ... }`               |
|   **空值检查**   |      `.null`       |        生成 `if (expr == null)` 检查        |     `currentUser`**.null**     |              `if (currentUser == null) { ... }`              |
|                  | `.notnull` / `.nn` |        生成 `if (expr != null)` 检查        |   `currentUser`**.notnull**    |              `if (currentUser != null) { ... }`              |
|   **类型操作**   |      `.cast`       |             将对象强制类型转换              |        `view`**.cast**         |           `((Type) view)` <br/>(光标在 Type 位置)            |
|                  |   `.instanceof`    |         生成 `instanceof` 类型检查          |      `obj`**.instanceof**      |  `if (obj instanceof Type) { ... }` <br/>(光标在 Type 位置)  |
|   **代码包裹**   |       `.try`       |        将语句包裹在 `try-catch` 块中        | `someRiskyOperation()`**.try** | `try { someRiskyOperation(); } catch (Exception e) { e.printStackTrace(); }` |
|                  |  `.synchronized`   |      将语句包裹在 `synchronized` 块中       |    `this`**.synchronized**     |                `synchronized (this) { ... }`                 |
|                  |      `.sout`       |    用 `System.out.println()` 打印表达式     |       `"Hello"`**.sout**       |                `System.out.println("Hello");`                |
|  **返回与抛出**  |     `.return`      |            从方法返回表达式的值             |    `buildUser()`**.return**    |                    `return buildUser();`                     |
|                  |      `.throw`      |          抛出一个 `Throwable` 异常          |  `new Exception()`**.throw**   |                   `throw new Exception();`                   |
| **Android 专用** |      `.toast`      |             显示一个 Toast 提示             |   `"Login failed"`**.toast**   | `Toast.makeText(context, "Login failed", Toast.LENGTH_SHORT).show();` |
|                  |      `.logd`       |       用 `Log.d` 打印 Debug 级别日志        |       `message`**.logd**       |                    `Log.d(TAG, message);`                    |
|                  |      `.loge`       |       用 `Log.e` 打印 Error 级别日志        |    `errorMessage`**.loge**     |        `Log.e(TAG, "errorMessage: ", errorMessage);`         |
|                  | `.logi` / `.logw`  |        用 `Log.i` / `Log.w` 打印日志        |        `info`**.logi**         |                     `Log.i(TAG, info);`                      |





## **Kotlin 后缀补全用法大全**





这份表格适用于在 `.kt` 文件中进行现代 Android 开发,充分利用了 Kotlin 的语言特性.

|            分类            |       模板       |                             说明                             |          输入示例          |                     生成代码                      |
| :------------------------: | :--------------: | :----------------------------------------------------------: | :------------------------: | :-----------------------------------------------: |
|        **变量声明**        |      `.val`      |                  创建一个不可变引用 (`val`)                  |  `User("guest")`**.val**   |            `val user = User("guest")`             |
|                            |      `.var`      |                   创建一个可变引用 (`var`)                   |        `0`**.var**         |                    `var i = 0`                    |
| **空安全与<br>作用域函数** |     `.null`      |                   检查表达式是否为 `null`                    |      `user`**.null**       |            `if (user == null) { ... }`            |
|                            |    `.notnull`    |                  检查表达式是否不为 `null`                   |     `user`**.notnull**     |            `if (user != null) { ... }`            |
|                            |    **`.let`**    |          安全调用，将非空对象作为 `it` 传入 lambda           |    `user?.name`**.let**    |             `user?.name?.let { ... }`             |
|                            |   **`.apply`**   |      安全调用，将非空对象作为 `this` 配置。返回对象本身      | `TextView(this)`**.apply** |          `TextView(this).apply { ... }`           |
|                            |   **`.also`**    |   安全调用，将非空对象作为 `it` 执行附加操作。返回对象本身   |      `user`**.also**       |                `user.also { ... }`                |
|                            |    **`.run`**    | 安全调用，将非空对象作为 `this` 执行 lambda。返回 lambda 结果 |  `user?.address`**.run**   |           `user?.address?.run { ... }`            |
|                            |     `.with`      |             使用 `with(expr) { ... }` 包裹代码块             |      `user`**.with**       |               `with(user) { ... }`                |
|          **循环**          | `.for` / `.iter` |                   遍历一个 `Iterable` 对象                   |     `userList`**.for**     |         `for (user in userList) { ... }`          |
|                            |     `.fori`      |                          带索引遍历                          |      `list`**.fori**       |         `for (i in list.indices) { ... }`         |
|                            |     `.forr`      |                        带索引反向遍历                        |      `list`**.forr**       |   `for (i in list.indices.reversed()) { ... }`    |
|       **条件与分支**       |      `.if`       |                 基于布尔表达式生成 `if` 语句                 |   `user.age > 18`**.if**   |           `if (user.age > 18) { ... }`            |
|                            |     `.else`      |             基于布尔表达式生成 `if (!expr)` 语句             | `list.isEmpty()`**.else**  |          `if (!list.isEmpty()) { ... }`           |
|                            |     `.when`      |                 为表达式生成 `when` 分支语句                 |  `response.code`**.when**  |          `when (response.code) { ... }`           |
|        **类型操作**        |     `.cast`      |                  智能类型转换并赋值给新变量                  |      `view`**.cast**       | `val type = view as Type` <br/>(光标在 Type 位置) |
|                            |      `.is`       |              生成 `if (expr is Type)` 类型检查               |      `animal`**.is**       |           `if (animal is Dog) { ... }`            |
|                            |     `.notis`     |              生成 `if (expr !is Type)` 类型检查              |     `animal`**.notis**     |           `if (animal !is Cat) { ... }`           |
|        **代码包裹**        |      `.try`      |                将语句包裹在 `try-catch` 块中                 |    `readFile()`**.try**    | `try { readFile() } catch (e: Exception) { ... }` |
|                            |     `.sout`      |                  用 `println()` 打印表达式                   | `"Hello Kotlin"`**.sout**  |             `println("Hello Kotlin")`             |
|       **返回与抛出**       |    `.return`     |                     从函数返回表达式的值                     | `buildResult()`**.return** |              `return buildResult()`               |





---



# 数组

## 一维数组

```java
// 快捷输入
new int[7].var
    
int[] a = new int[7];
```



## 二维数组

```java
// 快捷输入
new String[][].var
    
String[][] str = new String[][]{{"你好","hello"},{"世界","world"}};
```





---





# 面向对象

## 对象

对象是一种**存储数据**的特殊的数据结构,里面也可以包含对应的数据代码,new对象前你需要创建一个`class`类,它作为模版来创建具体的对象.



1.创建类

```java
// 这是一个“猫”的蓝图 (Class)
class Cat {
    // 状态 (属性)
    String name; // 名字
    int age;     // 年龄
    String color; // 颜色

    // 行为 (方法)
    void meow() {
        System.out.println(name + " says: 喵喵~");
    }

    void eat() {
        System.out.println(name + " 正在吃东西...");
    }
}
// 对象将自己的数据（状态）和操作数据的代码（行为）打包在一起，形成一个独立的单元,也体现了封装的思想
```



2. new对象

```java
public class Main {
    public static void main(String[] args) {
        // 创建第一个猫对象，名叫“小白”
        Cat cat1 = new Cat(); // new Cat() 就是在创建对象
        cat1.name = "小白";
        cat1.age = 2;
        cat1.color = "白色";

        // 创建第二个猫对象，名叫“阿橘”
        Cat cat2 = new Cat();
        cat2.name = "阿橘";
        cat2.age = 3;
        cat2.color = "橘色";

        // cat1 和 cat2 是两个完全独立的对象，尽管它们都来自同一个Cat类
        // 它们有各自的状态（名字、年龄等）
        System.out.println(cat1.name); // 输出: 小白
        System.out.println(cat2.name); // 输出: 阿橘

        // 我们可以让它们执行自己的行为
        cat1.meow(); // 输出: 小白 says: 喵喵~
        cat2.eat();  // 输出: 阿橘 正在吃东西...
    }
}
```





---



## 构造器

构造器（也常被称为构造方法、构造函数）是类中的一个特殊方法.它的**唯一目的**就是在创建一个类的对象时,对这个新创建的对象进行**初始化**.

我们可以把它想象成一个“生产车间”里的“初始化工序”.当使用 `new` 关键字生产一个新对象时,这个初始化工具立刻被调用,为对象的属性（成员变量）赋予初始值.



*特点：*

1. **名称必须与类名完全相同**：这是识别构造器的硬性标准.
2. **没有返回类型**：它连 `void` 都没有,因为它“返回”的其实是那个被初始化好的对象实例的引用,这个过程是隐式的.
3. **不能被直接调用**：你不能像调用普通方法那样（例如 `myCat.constructor()`) 来调用构造器.它只能在创建对象时,*由 `new` 关键字自动调用.*



*应用场景：*

完成对象的成员变量的初始化赋值



### 无参构造器 



这是最简单的构造器，它不接受任何参数。



```java
class Dog {
    String name;

    // 这是一个无参构造器
    public Dog() {
        System.out.println("一只小狗被创建了！");
        this.name = "旺财"; // 可以在这里设置默认值
    }
}

public class Main {
    public static void main(String[] args) {
        // 当执行 new Dog() 时，上面的无参构造器会被自动调用
        Dog dog1 = new Dog(); // 控制台会输出 "一只小狗被创建了！"
        System.out.println(dog1.name); // 输出 "旺财"
    }
}
```



### 有参构造器 



这种构造器接受一个或多个参数，使得我们可以在创建对象时就传递初始数据，实现更灵活的初始化。



```Java
class Cat {
    String name; // 成员变量
    int age;

    // 这是一个有参构造器，接受名字和年龄
    public Cat(String name, int age) { // 局部变量
        System.out.println("正在创建一只名叫 " + name + " 的猫...");
        this.name = name; // 使用 this 关键字区分成员变量和参数
        this.age = age;
    }
}

public class Main {
    public static void main(String[] args) {
        // 创建对象时，必须提供匹配的参数
        Cat cat1 = new Cat("咪咪", 2); // "咪咪"和 2 会被传递给构造器
        System.out.println("猫的名字是: " + cat1.name); // 输出 "猫的名字是: 咪咪"
    }
}
```



*注意:*

1. 类默认带了一个无参构造器.
2. 如果定义了个有参构造器,默认的无参构造器会消失,需要手动定义一个.



### 通过构造器传递对象

将通过构造函数参数传递进来的 `Movie` 对象,赋值给当前 `MovieOpearator` 实例的成员变量 `movie`,再调用构造方法输出数据.

```Java
/**
 * Movie 类：定义了电影的基本属性。
 */
class Movie {
    String title; // 电影标题
    String genre; // 电影类型

    // 构造器：用于创建并初始化一个 Movie 对象
    public Movie(String title, String genre) {
        this.title = title;
        this.genre = genre;
    }

    public String getTitle() {
        return this.title;
    }

    public String getGenre() {
        return this.genre;
    }
}

/**
 * MovieOperator 类：用于对一个 Movie 对象进行操作。
 */
class MovieOperator {
    // 持有一个私有的 Movie 实例
    private Movie movie;

    // 构造器：通过构造器注入所依赖的 Movie 对象
    public MovieOperator(Movie movie) {
        this.movie = movie;
    }

    public void setMovie(Movie movie) {
        this.movie = movie;
    }

    // 显示当前电影的详细信息
    public void displayMovieDetails() {
        System.out.println("--- 电影详情 ---");
        System.out.println("片名: " + movie.getTitle());
        System.out.println("类型: " + movie.getGenre());
    }
}

/**
 * Main 类：程序的执行入口。
 */
public class Main {
    public static void main(String[] args) {
        // 1. 创建一个 Movie 对象
        Movie inception = new Movie("盗梦空间", "科幻/悬疑");

        // 2. 创建 Operator 并注入 Movie 对象
        MovieOperator operator = new MovieOperator(inception);

        // 3. 调用方法，执行操作
        operator.displayMovieDetails();
    }
}
```



---





## this关键字

`this` 是一个关键字,也是一个引用,在任何方法或构造器内部,它都**指向调用该方法或构造器的当前对象**.



*特点：*

哪个对象调用带有`this`的方法,`this`就拿到哪个对象.



*应用场景:*

**1. 区分成员变量和局部变量（最常用的功能）**



当方法的参数名或局部变量名与类的成员变量名相同时，`this` 可以用来明确地指代**成员变量**。

**场景示例：** 在一个构造器或 Setter 方法中，我们通常希望参数名能清晰地描述其含义，所以会把它起得和成员变量名一样。



```Java
class Student {
    // 成员变量 (Instance Variable)
    String name;
    int age;

    // 构造器
    public Student(String name, int age) {
        
        // 正确的写法：使用 this 关键字
        // this.name 指的是当前Student对象的成员变量name
        // = 右边的 name 指的是传入的参数name
        this.name = name;
        this.age = age;
    }

    public void setName(String name) {
        // 同理，在setter方法中也常用
        this.name = name;
    }

    public void introduce() {
        // 在没有歧义的情况下，this可以省略
        // 下面这行代码等价于 System.out.println("我是" + this.name);
        System.out.println("我是" + name);
    }
}

public class Main {
    public static void main(String[] args) {
        Student zhangsan = new Student("张三", 20);
        System.out.println(zhangsan.name); // 输出: 张三
    }
}
```

小结：`this.成员变量 = 参数;` 是 `this` 最核心、最频繁的用法.

------



**2.在构造器中调用本类的其他构造器**



为了避免在重载的构造器中编写重复的初始化代码，可以使用 `this(...)` 来调用本类中的另一个构造器。

*规则：*

- `this(...)` 必须是构造器中的**第一条可执行语句**。
- 只能在构造器中使用。

**场景示例：** 假设我们有一个 `Rectangle` 类，希望提供多种创建方式。



```Java
class Rectangle {
    private int width;
    private int height;

    // 1. "全功能"构造器，负责实际的初始化工作
    public Rectangle(int width, int height) {
        this.width = width;
        this.height = height;
    }

    // 2. 创建一个正方形的构造器
    public Rectangle(int sideLength) {
        // 调用上面的双参数构造器，传入相同的边长
        this(sideLength, sideLength);
    }

    // 3. 无参构造器，创建一个默认大小的矩形
    public Rectangle() {
        // 调用上面的双参数构造器，传入默认值
        this(10, 5);
    }

    public void printInfo() {
        System.out.println("宽度: " + width + ", 高度: " + height);
    }
}

public class Main {
    public static void main(String[] args) {
        Rectangle r1 = new Rectangle(); // 调用无参构造器
        r1.printInfo(); // 输出: 宽度: 10, 高度: 5

        Rectangle r2 = new Rectangle(20); // 调用单参数构造器
        r2.printInfo(); // 输出: 宽度: 20, 高度: 20
    }
}
```

小结：通过 `this(...)` 实现构造器链，可以有效减少代码冗余。

------



**3.返回当前对象的引用**



在方法中可以使用 `return this;` 来返回调用该方法的对象本身,这种用法常用于实现**链式调用（Method Chaining）**,让代码写起来更流畅,常见于构建器模式.

设计一个简单的计算器，可以连续进行操作.



```Java
class Calculator {
    private int result = 0;

    public Calculator add(int num) {
        this.result += num;
        return this; // 返回计算器对象本身
    }

    public Calculator subtract(int num) {
        this.result -= num;
        return this; // 返回计算器对象本身
    }



    public void printResult() {
        System.out.println("最终结果是: " + this.result);
    }
}

public class Main {
    public static void main(String[] args) {
        Calculator calc = new Calculator();

        // 因为每个方法都返回了 calc 对象自身，所以可以连续调用
        calc.add(10).add(5).subtract(3);

        calc.printResult(); // 输出: 最终结果是: 12
    }
}
```

小结：`return this;` 是实现链式编程的关键.

------



**4. 将当前对象作为参数传递**



如果一个方法需要接收一个当前类的对象作为参数，可以在调用时直接传递 `this`。

**场景示例：** 一个事件监听器需要把自己注册到事件源中。



```Java
// 事件源
class EventSource {
    public void registerListener(EventListener listener) {
        listener.onEventHappened();
    }
}

// 事件监听器
class EventListener {
    // 这个方法把自己注册到事件源
    public void startListening(EventSource source) {
        // 把“我”这个EventListener对象传给source
        source.registerListener(this);
    }

    public void onEventHappened() {
        System.out.println("事件发生了，我（监听器）收到了！");
    }
}

public class Main {
    public static void main(String[] args) {
        EventSource source = new EventSource();
        EventListener listener = new EventListener();
        listener.startListening(source); // 监听器把自己注册进去
    }
}
```







| 用法             | 语法               | 目的                                                 |
| ---------------- | ------------------ | ---------------------------------------------------- |
| **区分变量**     | `this.variable`    | 明确指出要访问的是类的成员变量，而非同名局部变量。   |
| **调用构造器**   | `this(...)`        | 在一个构造器中调用本类的另一个构造器，减少代码重复。 |
| **返回当前对象** | `return this;`     | 从方法中返回当前对象实例，以支持链式调用。           |
| **传递当前对象** | `someMethod(this)` | 在方法调用中，将当前对象作为参数传递给另一个方法。   |







---





## 封装

封装是指将对象的**数据（属性）和操作数据的代码（方法）捆绑成一个独立的单元（即类）,并对对象的内部细节进行信息隐藏**,外部世界只能通过该对象允许的公开接口来访问它,而不能直接访问其内部的私有数据.

**设计要求:合理隐藏,合理暴露**

在 Java 中，实现封装通常遵循以下三个步骤：

1. **将属性设为私有（private）**：使用 `private` 关键字修饰类的成员变量,这样它们就不能在类的外部被直接访问.
2. **提供公有的 Getter 方法**：创建一个 `public` 的方法,用于读取某个私有属性的值（例如 `getName()`）.
3. **提供公有的 Setter 方法**：创建一个 `public` 的方法,用于设置某个私有属性的值（例如 `setName(String newName)`）,最关键的是,我们可以在这个方法中加入控制和验证逻辑.



```java
class Person {
    // 1. 将属性设为私有
    private String name;
    private int age;

    // 2. 提供公有的 Getter 方法，用于读取 name
    public String getName() {
        return this.name;
    }

    // 3. 提供公有的 Setter 方法，用于设置 name
    public void setName(String name) {
        this.name = name;
    }

    // 2. 提供公有的 Getter 方法，用于读取 age
    public int getAge() {
        return this.age;
    }

    // 3. 提供公有的 Setter 方法，用于设置 age
    public void setAge(int age) {
        if (age > 0 && age < 130) { // 只接受合法的年龄
            this.age = age;
        } else {
            System.out.println("错误：年龄值不合法！");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Person p = new Person();
        p.setName("李四");
        p.setAge(25); // 合法值，设置成功
        System.out.println(p.getName() + "的年龄是 " + p.getAge()); // 输出: 李四的年龄是 25

        p.setAge(-30); // 非法值，设置失败
        System.out.println(p.getName() + "的年龄是 " + p.getAge()); // 输出: 错误：年龄值不合法！                       
    }
}
```



*注意: `get`和`set`方法可以通过右键里面的`generate`生成.*

---





## 实体类/Javabean



它是一种特殊的技术规范或设计模式,可以把它想象成一个标准化的,自包含的数据封装容器,它的严格规范使得各种工具和框架都能自动地发现,使用和管理它的属性.

*特点:*

1. **公有的无参构造器**：必须提供一个 `public` 的、不带任何参数的构造函数。这是为了让很多框架（如 Spring, JSP）能够轻松地通过反射来创建它的实例。
2. **私有的成员变量**：所有属性（fields）都应该是 `private` 的，这体现了**封装**的原则。
3. **公有的 Getter 和 Setter 方法**：对每一个私有属性，都提供 `public` 的 `getXxx()` 和 `setXxx()` 方法用于读取和写入。对于布尔类型的属性，getter 方法可以是 `isXxx()`。
4. **可序列化（可选但推荐）**：实现 `java.io.Serializable` 接口。这使得 JavaBean 对象可以在网络上传输或持久化到文件中



```java
// 这是一个标准的 JavaBean
public class UserBean implements Serializable {

    // 2. 私有属性
    private String userName;
    private int age;

    // 1. 公有的无参构造器
    public UserBean() {
    }

    // 3. 公有的 Getter 和 Setter 方法
    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```





---







## static-静态变量/方法

`static` 关键字的核心思想是：**被 `static` 修饰的成员（变量或方法）属于整个类,而不是属于某个具体的对象实例.**

### static修饰成员变量

被`static`修饰的成员变量我们称为静态变量或者类变量,在计算机中只有一份,会被类的全部对象共享,



*特点:*

当一个变量被 `static` 修饰后，它就不再是对象的属性，而是类的属性。

- **内存分配**：`static` 变量在类被加载到内存时就分配了空间,并且只分配一次,无论创建了多少个对象,静态变量在内存中都只有一个副本.
- **调用方式**：推荐直接使用 **`类名.静态变量`** 的方式调用,虽然也可以用 `对象名.静态变量` 调用,但这会引起混淆,不推荐.



*应用场景*

在开发中,如果某个数据只需要一份,且希望能够被共享(访问,修改),则该数据可以定义成类变量来记住.





``` java
class Student {
    // 实例变量：每个学生对象都有一份
    String name;
    int studentId;

    // 静态变量：所有Student对象共享
    static String university = "北京大学";
}

public class Main {
    public static void main(String[] args) {
        // 直接通过类名访问静态变量
        System.out.println("学校是：" + Student.university);

        Student s1 = new Student();
        s1.name = "张三";

        Student s2 = new Student();
        s2.name = "李四";

        System.out.println(s1.name); // 输出: 张三
        System.out.println(s2.name); // 输出: 李四

        // 所有对象共享同一个静态变量
        System.out.println(s1.university); // 输出: 北京大学
        
        System.out.println(s2.university); // 输出: 北京大学

        // 修改静态变量会影响所有地方
        Student.university = "清华大学";
        System.out.println("s1的学校变为：" + s1.university); // 输出: s1的学校变为：清华大学
        System.out.println("s2的学校变为：" + s2.university); // 输出: s2的学校变为：清华大学
    }
}
```



---



### static修饰成员方法

与静态变量类似,静态方法是属于整个类的方法,而不是属于某个具体对象实例的方法.可以提高代码复用性与运行效率,不用创建对象,调用方便.

*规范:*

如果这个方法只是为了做一个功能且不需要直接访问,这个方法直接定义成静态方法.



*特点:*

- 静态方法内部不能直接调用非静态方法或访问非静态变量.
- 不能使用 `this` 或 `super` 关键字.



*应用场景*

- 自定义工具类**(记得私有化构造器)**
- 工厂方法



```java
// 字符串处理工具类
public final class StringUtils {
    // 私有化构造器，防止被实例化
    private StringUtils() {}

    // 判断字符串是否为空或null
    public static boolean isBlank(String str) {
        return str == null || str.trim().isEmpty();
    }

    // 将字符串首字母大写
    public static String capitalize(String str) {
        if (isBlank(str)) {
            return str;
        }
        return Character.toUpperCase(str.charAt(0)) + str.substring(1);
    }
}

// 使用
if (StringUtils.isBlank(userInput)) {
    System.out.println("输入不能为空！");
}
```





```java
// 1. 产品接口
interface Shape {
    void draw();
}

// 2. 具体产品类
class Circle implements Shape {
    @Override
    public void draw() {
        System.out.println("画了一个圆形 ○");
    }
}

class Rectangle implements Shape {
    @Override
    public void draw() {
        System.out.println("画了一个矩形 □");
    }
}

// 3. 工厂类
class ShapeFactory {
    // 提供一个公开的静态方法来获取Shape对象
    public static Shape getShape(String shapeType) {
        if (shapeType == null) {
            return null;
        }
        if (shapeType.equalsIgnoreCase("CIRCLE")) {
            return new Circle();
        } else if (shapeType.equalsIgnoreCase("RECTANGLE")) {
            return new Rectangle();
        }
        return null;
    }
}

public class Main {
    public static void main(String[] args) {
        // 客户端不需要知道 Circle 或 Rectangle 的存在，只和工厂打交道
        Shape shape1 = ShapeFactory.getShape("CIRCLE");
        shape1.draw(); // 输出: 画了一个圆形 ○

        Shape shape2 = ShapeFactory.getShape("RECTANGLE");
        shape2.draw(); // 输出: 画了一个矩形 □
    }
}
```





*注意:*

- 静态方法中可以直接访问静态成员,不可以直接访问实例成员(但可以在里面创建对象实现间接访问).
- 实例方法中既可以直接访问静态成员,也可以直接访问实例成员.
- 实例方法中可以出现`this`和`super`关键字,静态方法中不能出现(`this`代表的是对象,`super`代表的是父类).



---



## 权限修饰符

这张表格可以清晰地展示四个修饰符的访问权限范围：

| 修饰符          | 同一个类 | 同一个包 | 不同包的子类 | 任何地方 |
| --------------- | -------- | -------- | ------------ | -------- |
| **`private`**   | ✅        | ❌        | ❌            | ❌        |
| **`default`**   | ✅        | ✅        | ❌            | ❌        |
| **`protected`** | ✅        | ✅        | ✅            | ❌        |
| **`public`**    | ✅        | ✅        | ✅            | ✅        |



*注意:*

一般成员变量使用`private`,方法和函数使用`public`就可以了.



---





## 继承

继承的概念与现实世界中的生物遗传非常相似.

子女会从父母那里继承一些特征,比如眼睛的颜色、身高.同时,子女也会有自己独特的特征,比如一项新的爱好或技能.

在编程中：

- **子类可以从父类（基类）那里“继承”来属性（成员变量）和行为（方法）**.
- 子类不仅拥有父类的所有非私有成员,还可以添加自己独有的新属性和新方法,或者**重写（Override）**从父类继承来的方法,使其表现出不同的行为.



*特点*

- `Java`是单继承模式,不支持多继承.
- `Java`中所有的类都是`Object`的子类.
- 就近原则:会优先访问自己类中的局部变量,其次用`this`访问成员变量,访问父类的变量要用`super`.



```java
// 父类 Animal
public class Animal {
    String name;

    public Animal(String name) {
        this.name = name;
        System.out.println("Animal 的构造器被调用了！");
    }

    public void eat() {
        System.out.println(name + " 正在吃东西...");
    }

    public void sleep() {
        System.out.println(name + " 正在睡觉...");
    }
}

// 子类 Dog 继承自 Animal
public class Dog extends Animal {

    // Dog 类特有的方法
    public void bark() {
        System.out.println(name + " 汪汪叫！");
    }
}
```





### 访问成员-就进原则



“就近原则”指的是,当子类的方法中需要访问一个变量或调用一个方法时,程序会按照一个由近及远的顺序来查找这个成员,一旦找到了就会立刻停止查找并使用它.

这个查找顺序可以概括为：

1. **先在局部范围找**：首先在当前方法的局部变量中查找.
2. **再到当前对象找**：如果在方法内没找到,就在当前子类对象的成员变量中查找.
3. **最后去父类对象找**：如果在子类中也没找到，就沿着继承关系向上,到直接父类的成员变量中查找.如果父类还有父类,会继续向上,直到 `Object` 类为止.

```java
class Father {
    String name = "张伟"; // 成员变量

    public void sayHello() {
        System.out.println("我是父亲，我的名字是 " + name);
    }
}

class Son extends Father {
    String name = "张小宝"; // 与父类同名的成员变量

    public void showName() {
        String name = "临时名"; // 局部变量

        // 场景一：直接访问 name
        // 根据就近原则，首先找到的是方法内的局部变量 "临时名"
        System.out.println(name); 

        /** 
         * 场景二：使用 this 访问 name
         * this 关键字代表当前对象（Son对象），所以它会跳过局部变量，直接在当前类的成员变量中查找。
         * 找到了 Son 类中的 "张小宝"
         */
        System.out.println(this.name); 

        /** 
         * 场景三：使用 super 访问 name
         * super 关键字代表父类对象，所以它会跳过局部变量和当前子类成员，直接去父类中查找。
         * 找到了 Father 类中的 "张伟"
         */
        System.out.println(super.name); 
    }

    // 重写（Override）父类的 sayHello 方法
    @Override
    public void sayHello() {
        System.out.println("我是儿子，我的名字是 " + this.name);
    }
}
```



从上面的例子可以看出,`this` 和 `super` 是打破默认“就近”查找顺序、进行精确访问的关键工具.

- **`this` 关键字**
  - **含义**：代表当前对象的引用.
  - **作用**：用来明确访问当前类的成员变量或成员方法,当成员变量和局部变量同名时,必须使用 `this` 来区分.
- **`super` 关键字**
  - **含义**：代表对父类对象的引用.
  - **作用**：用来明确访问父类的成员变量或成员方法,当子类隐藏了父类的变量或重写了父类的方法后,可以用 `super` 来访问父类中的版本.

---



### 方法重写

也称为方法覆盖,指的是**在子类中创建一个与父类中某个方法具有相同方法签名（即方法名和参数列表完全相同）的新方法,从而覆盖掉父类的版本**.

简单来说,就是父类有一个通用的功能,但子类觉得这个功能的实现方式不适合自己,需要一个“定制版”或“特殊版”,于是子类就重新写了一遍这个功能.

**一个形象的比喻：**

- **父类 `Animal`** 有一个 `move()` 方法,它的实现可能是“移动身体”.
- **子类 `Fish`** 继承了 `Animal`,但鱼的移动方式很特殊，是“摇动尾巴游泳”.
- **子类 `Bird`** 也继承了 `Animal`,但鸟的移动方式是“扇动翅膀飞翔”.

在这里,`Fish` 和 `Bird` 都重写了父类 `Animal` 的 `move()` 方法,为其提供了更具体的实现.



*作用和好处*

方法重写是实现**多态性**的基,这是面向对象编程的三大特性（封装、继承、多态）之一.

其主要好处是：

1. **功能扩展和定制化**：子类可以在不修改父类代码的前提下,改变或扩展从父类继承来的方法的行为,使其更符合子类的特性.
2. **实现多态**：允许我们使用父类型的引用来指向子类型的对象,并在调用方法时,程序能够自动选择并执行子类重写后的方法.这使得代码更加灵活、可扩展和易于维护.



*规则*

要想正确地重写一个方法,必须遵循以下严格的规则.一个流行的记忆法是**“三同一小一大”**：

- **`三同` - 三个地方必须相同**

  1. **方法名必须相同**
  2. **参数列表必须相同**（参数的类型、数量、顺序都必须一致）
  3. *（方法名 + 参数列表 = 方法签名，所以核心是方法签名必须相同）*

- **`一小` - 返回值类型要更小或相等**

  1. **返回值类型**必须与父类中被重写方法的返回值类型**相同**,或者是其**子类型**(父类方法返回 `Animal`,子类重写后可以返回 `Animal` 或者 `Dog`（假设`Dog`是`Animal`的子类）).

- **`一大` - 访问权限要更大或相等**

  1. 子类重写方法的**访问修饰符**权限**不能低于**父类被重写方法的权限.

  2. 权限从大到小排序为：`public` > `protected` > `default` (包访问权限) > `private`(如果父类方法是 `protected`，子类重写时可以是 `protected` 或 `public`，但不能是 `default` 或 `private`).

     

*注意：*

- **`private` 方法不能被重写**：因为 `private` 方法对子类不可见.
- **`final` 方法不能被重写**：`final` 关键字修饰的方法表示这是最终版本，不希望被任何子类修改.
- **`static` 方法不能被重写**：静态方法属于类而不是对象.类可以定义同名的静态方法,但这被称为**隐藏（Hiding）**,而不是重写.

---



### 构造器的调用



####  this()：调用同一个类中的其他构造器



`this()` 用于在一个构造器中调用同一个类中的另一个重载的构造器.这在当你希望重用构造逻辑,避免代码重复时非常有用,**通过调用兄弟构造器,来对其初始化,类似于继承.**



*核心规则：*

- **必须是构造器的第一行语句**：`this()` 调用必须是构造器中的第一个可执行语句。
- **只能在构造器中使用**：你不能在普通方法中使用 `this()` 来调用构造器。
- **不能形成递归调用**：两个或多个构造器之间不能通过 `this()` 相互调用，否则会造成无限循环，导致编译错误。



```Java
public class Person {
    private String name;
    private int age;

    // 1. 无参构造器
    public Person() {
        // 调用下面的有两个参数的构造器，并提供默认值
        this("Unknown", 0);
        System.out.println("无参构造器被调用");
    }

    // 2. 只带一个 name 参数的构造器
    public Person(String name) {
        // 调用下面的有两个参数的构造器，年龄使用默认值
        this(name, 0);
        System.out.println("一个参数的构造器被调用");
    }

    // 3. 带有 name 和 age 两个参数的构造器 (主要的初始化逻辑)
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
        System.out.println("两个参数的构造器被调用 (核心初始化)");
    }

    public void display() {
        System.out.println("Name: " + this.name + ", Age: " + this.age);
    }

    public static void main(String[] args) {
        System.out.println("--- 创建 p1 ---");
        Person p1 = new Person(); // 调用无参构造器
        p1.display();

        System.out.println("\n--- 创建 p2 ---");
        Person p2 = new Person("Alice"); // 调用一个参数的构造器
        p2.display();
    }
}
/*
--- 创建 p1 ---
两个参数的构造器被调用 (核心初始化)
无参构造器被调用
Name: Unknown, Age: 0

        --- 创建 p2 ---
两个参数的构造器被调用 (核心初始化)
一个参数的构造器被调用
Name: Alice, Age: 0*/
```



- 创建 `p1` 时，`new Person()` 调用了无参构造器。该构造器第一行 `this("Unknown", 0)` 将调用权转交给了 `Person(String, int)` 构造器。
- `Person(String, int)` 执行完核心的赋值操作后，控制权返回到无参构造器，继续执行后面的打印语句。
- 这种模式被称为**构造器链 (Constructor Chaining)**，它将初始化逻辑集中在一个构造器中，使得代码更易于维护。
- 相当于构造器不用重写

```java
 // this.name = name;
 // this.age = age;
 // System.out.println("两个参数的构造器被调用 (核心初始化)");
```



------



#### super()：调用父类的构造器



`super()` 用于在子类的构造器中显式地调用其直接父类的构造器,**子类的所有构造器都会先调用父类的构造器再执行自己,确保父对象的部分得到正确的初始化**.



*核心规则*

- **隐式调用**：如果你在子类的构造器中没有显式地使用 `super()` 或 `this()`，那么Java编译器会自动在构造器的第一行插入一个无参的 `super()` 调用，即 `super();`。
- **父类必须有可访问的无参构造器**：如果父类没有提供无参构造器（例如，父类只定义了有参构造器），那么在子类的构造器中**必须**显式地使用 `super()` 来调用父类某个存在的有参构造器，否则会产生编译错误。
- **必须是构造器的第一行语句**：和 `this()` 一样，`super()` 调用也必须是子类构造器中的第一个可执行语句。



```Java
// 父类
class Animal {
    String name;

    // 父类的构造器
    public Animal(String name) {
        this.name = name;
        System.out.println("Animal 的构造器被调用");
    }
}

// 子类
class Dog extends Animal {
    String breed;

    // 子类的构造器
    public Dog(String name, String breed) {
        // 显式调用父类带有一个 String 参数的构造器
        // 必须是第一行
        super(name);

        this.breed = breed;
        System.out.println("Dog 的构造器被调用");
    }

    public void display() {
        System.out.println("Name: " + this.name + ", Breed: " + this.breed);
    }

    public static void main(String[] args) {
        Dog myDog = new Dog("Buddy", "Golden Retriever");
        myDog.display();
    }
}
/*
Animal 的构造器被调用
Dog 的构造器被调用
Name: Buddy, Breed: Golden Retriever*/
```



1. 当我们创建 `Dog` 对象时 `new Dog(...)`,`Dog` 的构造器被调用/
2. `Dog` 构造器的第一行是 `super(name)`/这会立即调用父类 `Animal` 中匹配的构造器（即 `Animal(String name)`）,并把 "Buddy" 传递过去/
3. `Animal` 的构造器执行,初始化 `name` 属性,并打印消息/
4. 父类构造器执行完毕后,控制权返回到 `Dog` 的构造器,继续执行 `this.breed = breed;` 和后面的打印语句/

这个过程确保了在初始化子类特有属性（如 `breed`）之前,从父类继承来的属性（如 `name`）已经被正确地初始化了/







|     特性     | `this()`                                                     | `super()`                                                    |
| :----------: | ------------------------------------------------------------ | ------------------------------------------------------------ |
|   **目的**   | 调用**同一个类中**的另一个重载构造器/                        | 调用**直接父类**的构造器.                                    |
|   **位置**   | 必须是构造器的**第一行语句**/                                | 必须是构造器的**第一行语句**.                                |
|   **共存**   | `this()` 和 `super()` **不能**在同一个构造器中同时出现,因为它们都要求自己是第一行语句. |                                                              |
| **隐式调用** | 不会自动调用.                                                | 如果没有显式调用 `this()` 或 `super()`,编译器会自动在第一行插入一个无参的 `super()`. |
| **使用场景** | 代码复用，将多个构造器的初始化逻辑引导到一个主构造器中.      | 初始化子对象时,确保父对象的部分得到正确的初始化,这是继承中构造过程的必要环节. |









---







## 多态

多态是继承/实现情况下的一种现象,表现为: 对象多态和行为多态.**它的核心思想是：同一操作作用于不同的对象,可以有不同的解释,产生不同的执行结果.**





### 多态的基本介绍

**实现多态的三个必要条件**

在大多数主流编程语言中,要实现多态,通常需要满足以下三个条件：

1. **要有继承.：** 必须存在父类和子类的关系.
2. **要有方法重写：** 子类必须重写父类的某个方法.
3. **要有父类引用指向子类对象.：** 在使用时,通过父类的引用变量来引用子类的实例.

例如，在Java中：`Animal animal = new Dog();`

- `Animal` 是父类（引用变量的声明类型）
- `Dog` 是子类（对象的实际类型）
- `animal` 是一个父类引用，但它指向了一个 `Dog` 对象



多态主要分为两种：

1. **编译时多态/ 静态多态/ 对象多态(只接受父类方法,编译看左边)：**
   - 通过**方法重载**实现。
   - 方法重载指的是在同一个类中，可以有多个同名的方法，但它们的参数列表（参数的个数、类型或顺序）不同。
   - 编译器在编译阶段，就会根据你传入的参数来决定具体调用哪个方法。
2. **运行时多态/ 动态多态/ 行为多态(接受不同子类的方法,运行看右边)：**
   - 这是我们通常所说的多态，通过**方法重写**实现。
   - 在程序运行时，系统根据对象的实际类型来决定调用哪个方法。
   - 上面的代码示例就是典型的运行时多态。



```java
// 父类：形状
class Shape {
    public void draw() {
        System.out.println("绘制一个形状");
    }
}

// 子类：圆形
class Circle extends Shape {
    // 重写父类的 draw 方法
    @Override
    public void draw() {
        System.out.println("绘制一个圆形 ○");
    }

    // Circle类特有的方法
    public void roll() {
        System.out.println("圆形正在滚动...");
    }
}

// 子类：矩形
class Rectangle extends Shape {
    // 重写父类的 draw 方法
    @Override
    public void draw() {
        System.out.println("绘制一个矩形 □");
    }

    // Rectangle类特有的方法
    public double calculateArea(double width, double height) {
        return width * height;
    }
}
```





**对象多态**

```java
public class ObjectPolymorphismDemo {

    public static void main(String[] args) {
        System.out.println("--- 对象多态示例 ---");

        // 1. 创建一个 Circle 对象，并用 Circle 类型的引用指向它
        // 这是最常规的用法，不涉及多态
        Circle circleObject = new Circle();
        
        // 2. 创建一个 Circle 对象，并用 Shape 类型的引用指向它
        // 这就是“对象多态”的核心体现
        // shapeRef 的“声明类型”是 Shape，“实际类型”是 Circle
        Shape shapeRef = new Circle();

        // 3. 通过父类引用，我们可以调用父类中定义的方法
        // 尽管 shapeRef 实际指向 Circle，但编译器认为它是一个 Shape
        shapeRef.draw(); // 合法调用

        // 4. 【关键点】通过父类引用，我们“不能”调用子类特有的方法
        // 下面这行代码会导致“编译错误”！
        // 因为编译器检查 shapeRef 的类型是 Shape，而 Shape 类没有 roll() 方法。
        // 这就展示了对象多態在编译阶段的类型限制。
        // shapeRef.roll(); // -> 编译错误：方法 roll() 在类型 Shape 中未定义

        System.out.println("shapeRef 的引用类型是 Shape, 但它实际指向了一个 Circle 对象。");
        System.out.println("这种身份上的多重性，就是对象多态。");
    }
}
```





**行为多态**

```java
public class BehaviorPolymorphismDemo {

    // 定义一个静态方法，参数是父类 Shape 类型
    // 这个方法可以接受任何 Shape 的子类对象，体现了多态的扩展性
    public static void executeDraw(Shape s) {
        System.out.print("准备执行绘制动作... ");
        s.draw(); // 核心：同样的一行代码，行为却不同
    }

    public static void main(String[] args) {
        System.out.println("--- 行为多态示例 ---");

        // 创建两个不同的子类对象
        Shape myCircle = new Circle();       // 对象多态：用父类引用指向子类对象
        Shape myRectangle = new Rectangle(); // 对象多态：用父类引用指向子类对象

        // 调用同一个 executeDraw 方法，传入不同的对象
        executeDraw(myCircle);
        executeDraw(myRectangle);
        
        System.out.println("\n--- 在数组中演示 ---");
        // 将不同子类对象放入一个父类类型的数组中
        Shape[] shapes = {new Circle(), new Rectangle(), new Shape()};

        // 循环遍历数组，对每个元素执行 draw()
        for (Shape currentShape : shapes) {
            currentShape.draw(); // 运行时，JVM会判断 currentShape 的真实身份并调用对应的方法
        }
    }
}
```





我们可以用一个简单的因果关系来理解它们：

**因为** 有了 **对象多态**（一个 `Dog` 对象可以被看作是 `Animal`）， **所以** 才能实现 **行为多态**（当这个被看作 `Animal` 的 `Dog` 对象执行 `makeSound` 动作时，它表现出 `Dog` 的行为）。

|     特性     | 对象多态                                 | 行为多态                              |
| :----------: | ---------------------------------------- | ------------------------------------- |
| **核心概念** | 对象身份的多样性 (一个对象, 多种类型)    | 方法行为的多样性 (一个接口, 多种实现) |
| **关键机制** | 向上转型                                 | 方法重写  和 动态绑定                 |
| **作用阶段** | 编译期进行类型检查，运行期进行对象实例化 | 运行期决定调用哪个方法                |
|  **关注点**  | 对象**是什么** (Is-A relationship)       | 对象**做什么** (How it behaves)       |
|   **关系**   | **前提 / 基础**                          | **结果 / 体现**                       |



*注意*

多态是对象的知识,和成员变量无关.

---



### 多态的好处及其存在的问题

#### 好处

* 在多态形式下,右边对象是解耦合的(可以拆分与组装),更便于扩展和维护.

```java
People p1 = new Student();
People p1 = new Teacher();

p1.run();
```



* 定义方法时,使用父类类型的形参,可以接受一切子类对象,扩展性更强,更便利.

```java
Wolf w = new Wolf();
go(w);

Tortoise t = new Tortoise();
go(t);

public static void go(Animal a){
    System.out.println("开始");
    a.run();
}
```





#### 问题

* 多态下不能使用子类的独有功能(父类没有重写的).

```java
class Animal {
    public void eat() {
        System.out.println("动物在吃东西");
    }
}

class Dog extends Animal {
    @Override
    public void eat() {
        System.out.println("狗在啃骨头");
    }

    // Dog的独有方法
    public void bark() {
        System.out.println("汪汪汪！");
    }
}

class Cat extends Animal {
    @Override
    public void eat() {
        System.out.println("猫在吃鱼");
    }

    // Cat的独有方法
    public void meow() {
        System.out.println("喵喵喵~");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal pet = getPet("dog"); // getPet方法返回一个Animal引用，实际可能是Dog或Cat

        // 我们可以安全地调用eat()，因为Animal类里有定义
        pet.eat(); // 运行时会根据pet的实际类型决定调用Dog还是Cat的eat()方法

        // 尝试调用Dog的独有方法
        // pet.bark(); // ！！！这里会产生编译错误 ！！！
    }

    public static Animal getPet(String petType) {
        if ("dog".equals(petType)) {
            return new Dog();
        } else {
            return new Cat();
        }
    }
}
```



编译器在检查 `pet.bark()` 时,它只知道 `pet` 是一个 `Animal` 类型的引用,它无法在编译阶段预知 `getPet()` 方法在运行时到底会返回一个 `Dog` 对象还是一个 `Cat` 对象.

- 如果返回的是 `Dog`,调用 `bark()` 没问题.
- 但如果返回的是 `Cat`,`Cat` 对象根本没有 `bark()` 方法,运行时就会出错（`MethodNotFound` 之类的错误）.



**解决方法**

既然编译器是因为不确定引用的真实类型才阻止我们,那么我们只需要向编译器“证明”这个引用的真实类型是安全的,就可以调用了,这个“证明”的过程就是 **强制类型转换（向下转型）**.

但是,在转换之前，做一个负责任的程序员,我们应该先用 `instanceof` 来检查一下,确保转换是安全的，避免 `ClassCastException`.

```Java
public static void main(String[] args) {
    Animal pet = getPet("dog");
    pet.eat(); // 输出: 狗在啃骨头

    // 想要调用bark()，需要先判断再强转
    if (pet instanceof Dog) {
        Dog realDog = (Dog) pet; // 向下转型
        realDog.bark(); // 现在可以安全地调用了。输出: 汪汪汪！
    } else if (pet instanceof Cat) {
        Cat realCat = (Cat) pet;
        realCat.meow();
    }
}
```

通过 `if (pet instanceof Dog)`,我们向编译器和JVM都明确了一件事：在这个代码块内部,`pet` 确实指向一个 `Dog` 对象,因此可以安全地将其转换为 `Dog` 类型,并调用其独有的 `bark()` 方法.





---







# final关键字

final关键字是最终的意思,可以修饰:类,方法,变量.

* 修饰类: 该类被称为最终类,特点是不能被继承.(工具类)
* 修饰方法: 该方法被称为最终方法,特点是不能被重写了.
* 修饰变量: 该变量有且仅能被赋值一次.(定义常量)

```java
/**
 * 应用全局常量类
 * final 关键字确保这个类不能被继承。
 * 私有的构造函数防止外部通过 new AppConstants() 来创建实例。
 */
public final class AppConstants {

    /**
     * 私有构造函数，防止实例化
     */
    private AppConstants() {
        // 这个构造函数永远不会被调用
    }

    // --- 应用基本信息 ---
    public static final String APP_NAME = "My Awesome App";
    public static final String APP_VERSION = "1.0.2";

    // --- 网络配置 ---
    public static final String API_BASE_URL = "https://api.example.com/v1/";
    public static final int DEFAULT_TIMEOUT_SECONDS = 30; // 单位：秒
    public static final int MAX_RETRIES = 3;

    // --- 数据库配置 ---
    public static final String DB_NAME = "user_database";
    public static final int DB_VERSION = 2;

    // --- 默认配置 ---
    public static final String DEFAULT_LANGUAGE = "zh-CN";
    public static final int DEFAULT_PAGE_SIZE = 20;

}
```







| 修饰目标 | 效果                                                         | 主要目的                                          |
| -------- | ------------------------------------------------------------ | ------------------------------------------------- |
| **变量** | **一次赋值,终身不变**。<br>- 基本类型：值不变.<br>- 引用类型：指向的地址不变,但对象内容可变. | 创建常量,保证线程安全,实现不可变对象.             |
| **方法** | **不能被子类重写 (Override)**.                               | 锁定核心逻辑,防止子类破坏原有设计,保证程序稳定性. |
| **类**   | **不能被继承 (Inherit)**.                                    | 保证类的实现不会被修改,安全性,设计完整性.         |







---





# 单例类







单例模式是一种创建型设计模式,它确保一个类只有一个实例,并提供一个全局访问点来获取这个唯一的实例.

这种模式在很多场景下都非常有用,比如：

- **应用上下文管理**：提供一个全局、安全的 Context,避免内存泄漏.
- **数据库/Repository**：保证全局使用唯一的数据库连接,避免资源浪费和性能开销.
- **网络请求客户端**：复用底层的线程池、连接池和缓存,提高网络请求效率.
- **`SharedPreferences `工具类**：统一管理数据读写,方便在应用各处调用.

*特点：*

1. **私有的构造函数 (Private Constructor)**：为了防止外部通过 `new` 关键字直接创建类的实例,需要将构造函数声明为私有的.
2. **私有的静态实例变量 (Private Static Instance Variable)**：在类的内部创建一个该类自身的静态实例.
3. **公有的静态工厂方法 (Public Static Factory Method)**：提供一个全局的、静态的方法,用于返回这个唯一的实例,这个方法通常被命名为 `getInstance()`.

------







## 饿汉式

饿汉式在类加载的时候就直接创建实例,不管你是否需要它.

```Java
public class EagerSingleton {

    // 1. 私有的静态实例，在类加载时就进行初始化
    private static final EagerSingleton instance = new EagerSingleton();

    // 2. 私有的构造函数
    private EagerSingleton() {}

    // 3. 公有的静态方法，返回已经创建好的实例
    public static EagerSingleton getInstance() {
        return instance;
    }
}
```

- **优点**：
  - 实现简单.
  - 线程安全.因为 JVM 在加载类时,静态变量的初始化过程是线程安全的.
- **缺点**：
  - 资源浪费.如果这个实例从始至终都没有被使用,那么它所占用的内存就浪费了.



## 懒汉式

懒汉式在第一次调用 `getInstance()` 方法时才创建实例.

### 基础懒汉式（线程不安全）

```Java
public class LazySingleton {

    private static LazySingleton instance;

    private LazySingleton() {}

    public static LazySingleton getInstance() {
        // 在第一次使用时才创建实例
        if (instance == null) {
            instance = new LazySingleton();
        }
        return instance;
    }
}
```

- **优点**：
  - 实现了懒加载,避免了资源浪费.
- **缺点**：
  - **线程不安全**.在多线程环境下,可能会有多个线程同时进入 `if (instance == null)` 判断,从而创建出多个实例,违背了单例的初衷.**因此,这种方式不推荐在多线程环境中使用.**



###  同步方法懒汉式（线程安全）

为了解决线程不安全问题,最直接的方法就是给 `getInstance()` 方法加上 `synchronized` 关键字.

```Java
public class SynchronizedLazySingleton {

    private static SynchronizedLazySingleton instance;

    private SynchronizedLazySingleton() {}

    // 使用 synchronized 关键字保证线程安全
    public static synchronized SynchronizedLazySingleton getInstance() {
        if (instance == null) {
            instance = new SynchronizedLazySingleton();
        }
        return instance;
    }
}
```

- **优点**：
  - 线程安全.
  - 实现了懒加载.
- **缺点**：
  - **性能低下**.`synchronized` 会给整个方法加锁,每次调用 `getInstance()` 都会进行同步,但实际上只有在第一次创建实例时才需要同步.一旦实例创建完毕,后续的同步就成了不必要的开销,会严重影响性能.



### 双重检查锁定

这是对同步方法懒汉式的一种优化,在保证线程安全的同时,也兼顾了性能.

```Java
public class DoubleCheckedLockingSingleton {

    // 使用 volatile 关键字确保多线程下的可见性和禁止指令重排
    private static volatile DoubleCheckedLockingSingleton instance;

    private DoubleCheckedLockingSingleton() {}

    public static DoubleCheckedLockingSingleton getInstance() {
        // 第一次检查，避免不必要的同步
        if (instance == null) {
            // 同步块，只在实例未创建时进行同步
            synchronized (DoubleCheckedLockingSingleton.class) {
                // 第二次检查，确保只有一个线程创建实例
                if (instance == null) {
                    instance = new DoubleCheckedLockingSingleton();
                }
            }
        }
        return instance;
    }
}
```

- **注意**：`instance` 变量必须用 `volatile` 关键字修饰.这是因为 `instance = new DoubleCheckedLockingSingleton();` 这行代码在 JVM 中并非原子操作,它大致可以分为三步：
  1. 为 `instance` 分配内存空间.
  2. 初始化 `instance` 对象.
  3. 将 `instance` 变量指向分配的内存地址. JVM 可能会进行指令重排序,导致步骤 3 在步骤 2 之前执行.如果一个线程执行了 1 和 3,但还没执行 2,另一个线程在第一次检查 `if (instance == null)` 时会发现 `instance` 不为 `null`,然后直接返回一个未完全初始化的对象,从而导致问题.`volatile` 可以禁止这种指令重排.
- **优点**：
  - 线程安全.
  - 性能较高,只有在第一次创建时才会同步.
  - 实现了懒加载.
- **缺点**：
  - 实现相对复杂.

## 静态内部类 

这是一种被广泛推荐的实现方式,它结合了懒汉式和饿汉式的优点.

```Java
public class StaticInnerClassSingleton {

    private StaticInnerClassSingleton() {}

    // 静态内部类
    private static class SingletonHolder {
        private static final StaticInnerClassSingleton INSTANCE = new StaticInnerClassSingleton();
    }

    public static StaticInnerClassSingleton getInstance() {
        return SingletonHolder.INSTANCE;
    }
}
```

- **原理**：
  - 当 `StaticInnerClassSingleton` 类被加载时,它的静态内部类 `SingletonHolder` 并不会被加载.
  - 只有当第一次调用 `getInstance()` 方法时,JVM 才会加载 `SingletonHolder` 类,并初始化 `INSTANCE` 静态变量.
  - JVM 在类加载时保证了初始化的线程安全性.
- **优点**：
  - 线程安全.
  - 实现了懒加载.
  - 实现简单，代码清晰.



## 枚举

这是《Effective Java》作者 Joshua Bloch 极力推荐的方式,也是最简单、最安全的实现方式.

```Java
public enum EnumSingleton {
    INSTANCE;

    public void doSomething() {
        System.out.println("Doing something...");
    }
}

public class Main {
    public static void main(String[] args) {
        // 1. 直接通过 EnumSingleton.INSTANCE 获取唯一的实例
        EnumSingleton instance1 = EnumSingleton.INSTANCE;

        // 2. 调用实例的方法
        instance1.doSomething(); // 输出: Doing something...

        // 3. 再次获取实例，验证是否为同一个对象
        EnumSingleton instance2 = EnumSingleton.INSTANCE;

        // 比较 instance1 和 instance2 的内存地址
        // 结果会是 true，证明了它们是同一个实例
        System.out.println(instance1 == instance2); // 输出: true
    }
}
```

使用时,直接通过 `EnumSingleton.INSTANCE` 来访问.



```java
public enum TrafficLight {
    RED, YELLOW, GREEN
}

public class TrafficController {
    public void handleLight(TrafficLight currentLight) {
        switch (currentLight) {
            case RED:
                System.out.println("停止！");
                break;
            case YELLOW:
                System.out.println("减速，准备停车。");
                break;
            case GREEN:
                System.out.println("通行！");
                break;
            // 不需要 default，因为所有枚举情况都已覆盖。
            // 如果未来给 TrafficLight 增加了新的常量，编译器会警告 switch 语句不完整。
        }
    }

    public static void main(String[] args) {
        TrafficController controller = new TrafficController();
        controller.handleLight(TrafficLight.RED); // 输出: 停止！
    }
}
```

做信息分类和标志.



- **优点**：
  - **实现极其简单**.
  - **线程安全**.由 JVM 从根本上保证了线程安全.
  - **能防止反序列化重新创建新的对象**.Java 的序列化机制允许通过反序列化创建一个新的对象,但对于枚举类,JVM 会保证即使反序列化也不会创建新的实例.而其他方式实现的单例类需要特殊处理才能防止反序列化破坏单例.
  - **能防止反射攻击**。其他方式可以通过反射调用私有构造函数来创建新实例,而枚举类则可以防止这种情况.
- **缺点**：
  - 非懒加载.
  - 可读性可能稍差,对于不熟悉枚举单例的开发者可能需要时间理解.





| 实现方式               | 线程安全 | 懒加载 | 推荐程度 | 备注                                  |
| ---------------------- | -------- | ------ | -------- | ------------------------------------- |
| **饿汉式**             | 是       | 否     | ⭐⭐⭐      | 简单，但可能浪费资源                  |
| **懒汉式（基础）**     | 否       | 是     | ⭐        | **不推荐使用**                        |
| **懒汉式（同步方法）** | 是       | 是     | ⭐⭐       | 性能差，不推荐使用                    |
| **双重检查锁定**       | 是       | 是     | ⭐⭐⭐⭐     | 推荐，但实现略复杂，需注意 `volatile` |
| **静态内部类**         | 是       | 是     | ⭐⭐⭐⭐⭐    | **强烈推荐**，兼顾性能和简洁          |
| **枚举**               | 是       | 否     | ⭐⭐⭐⭐⭐    | **强烈推荐**，最简单、最安全的方式    |





---





# 抽象类/方法

## 抽象方法

抽象方法是指一个**只有方法声明,没有具体方法体**（即没有 `{}` 代码块）的方法.它存在的意义是定义一个规范或契约,告诉子类“你必须实现这个功能”,但具体怎么实现,由子类自己决定.

**语法特征**：

1. 使用 `abstract` 关键字进行修饰.
2. 没有方法体,直接以分号 `;` 结束.

```java
public abstract class Vehicle {
    // 这是一个抽象方法，它没有具体的实现
    public abstract void startEngine(); 
    
    // 这不是抽象方法，它有自己的实现
    public void turnOff() {
        System.out.println("引擎已关闭。");
    }
}
```

在这个例子中，`startEngine()` 就是一个抽象方法.它规定了所有 `Vehicle`（交通工具）都必须有“启动引擎”这个功能,但汽车（Car）和摩托车（Motorcycle）启动引擎的方式可能不同,所以具体实现留给它们各自的子类.

------



## 抽象类

抽象类是指一个**包含抽象方法的类**.只要一个类里哪怕只有一个抽象方法,这个类就必须被声明为抽象类.抽象类就像一个“半成品”或“模板”,它不能被直接实例化（即不能用 `new` 关键字创建对象）,因为它包含未实现的功能.它的唯一用途就是被其他类继承.

**核心特点**：

1. **不能被实例化**：你不能写 `new Vehicle()` 这样的代码,因为 `Vehicle` 是一个抽象类,它的 `startEngine()` 方法还没有实现,直接创建对象没有意义.
2. **必须被继承**：抽象类存在的目的就是为了让子类来继承它,并实现它所有的抽象方法.
3. **可以包含非抽象方法**：抽象类不仅可以有抽象方法,也可以有已经实现了的具体方法（如上面的 `turnOff()` 方法）.这使得子类可以复用这些通用功能**(用`final `关键字修饰)**.
4. **可以包含成员变量、构造函数等**：抽象类和普通类一样,可以拥有成员变量和构造函数.其构造函数主要是为了方便子类在构造时调用 `super()` 来初始化父类的成员.
5. **子类的责任**：
   - 一个类如果继承了抽象类,那么它**必须实现父类中所有（未被实现的）抽象方法**.
   - 如果子类不想实现父类的所有抽象方法,那么这个**子类必须被声明为抽象类**.

```Java
// Car 是一个具体的类，它继承了抽象类 Vehicle
public class Car extends Vehicle {

    // 必须实现父类中所有的抽象方法
    @Override
    public void startEngine() {
        System.out.println("汽车：插入钥匙，转动点火。");
    }
}

    
// Motorcycle 也是一个具体的类
public class Motorcycle extends Vehicle {

    @Override
    public void startEngine() {
        System.out.println("摩托车：脚踩启动杆。");
    }
}

public class Main {
    public static void main(String[] args) {
        // Vehicle vehicle = new Vehicle(); // 错误！不能实例化抽象类

        Vehicle myCar = new Car();         // 正确，使用子类实例化
        Vehicle myMotorcycle = new Motorcycle(); // 正确

        myCar.startEngine();        // 调用 Car 类自己实现的方法
        myCar.turnOff();            // 调用从 Vehicle 类继承来的通用方法

        myMotorcycle.startEngine(); // 调用 Motorcycle 类自己实现的方法
        myMotorcycle.turnOff();     // 调用从 Vehicle 类继承来的通用方法
    }
}
```



------



## 抽象类/方法的作用

抽象类的主要目的是为了**代码复用**和**强制规范**.

1. **代码复用 (Code Reusability)**： 当多个子类有一些共同的功能时,可以将这些功能实现在抽象父类中（作为具体方法）,子类只需要继承就可以直接使用,避免了在每个子类中重复编写相同的代码.例如上面例子中的 `turnOff()` 方法.
2. **强制规范 (Enforce a Contract)**： 通过抽象方法,父类可以为所有子类定义一个统一的接口或模板.它强制要求所有子类都必须提供某个特定功能的实现,从而保证了体系内所有对象都具有某些基本行为,这使得多态（Polymorphism）的应用更加安全和方便.例如,我们能确保任何 `Vehicle` 类型的对象,都可以调用 `startEngine()` 方法,而不用担心这个方法不存在.



---





# 接口

接口(Interface)是一种行为规范或契约,它一般只**定义一种方法签名(方法名,参数,返回值类型),但不提供任何具体的实现.**如果任何类实现了这个接口,那么它必须要为方法提供**具体的实现代码.**



你可以把接口想象成一份合同或一个设备的插座标准：

- **合同**：合同规定了甲方和乙方需要履行的义务（必须做什么），但没有规定具体怎么做。任何签署合同的人都必须履行这些义务。
- **USB 插座**：USB 接口标准规定了插座的形状、引脚定义和功能。任何设备（U盘、鼠标、键盘）只要遵循这个标准，就能插入USB端口并正常工作。电脑不关心你插的是什么设备，只关心它是否符合USB规范。



*特点与优势*

1. **纯粹的抽象**：接口是完全抽象的,它只包含方法的声明,不包含方法的实现.*注：现代编程语言如 Java 8+ 允许接口包含 `private`,`default`和 `static` 方法,但这属于特例,其核心思想仍是定义规范.*
2. **不能被实例化**：你不能直接用 `new` 关键字创建一个接口的对象,因为它没有具体的功能实现.
3. **必须被类实现**：接口的价值在于被类（Class）来实现,一个类可以实现一个或多个接口.
4. **强制实现**：实现接口的类必须为接口中的所有方法提供具体的实现,否则,这个类必须被声明为抽象类.
5. **多实现**：一个类只能继承一个父类（单继承），但可以**实现多个接口**.这解决了单继承的局限性,使得一个类可以同时拥有多种“能力”.
6. **多继承**: 一个接口可以继承多个接口.
7. **解耦合**: 让程序可以面向接口编程,程序员可以切换各种任务实现.



```java
// 定义一个“可飞行的”接口
public interface Flyable {
    
    // 接口中的方法默认是 public abstract 的，所以可以省略这两个关键字
    void takeOff(); // 起飞
    
    void fly();     // 飞行
    
    void land();    // 降落
}

public class Bird implements Flyable {

    @Override
    public void takeOff() {
        System.out.println("鸟：扇动翅膀，蹬腿起飞。");
    }

    @Override
    public void fly() {
        System.out.println("鸟：在空中翱翔。");
    }

    @Override
    public void land() {
        System.out.println("鸟：收拢翅膀，平稳着陆。");
    }
}
```





## 接口的主要用途



1. **实现多态接口使得我们可以编写更通用、更灵活的代码,我们可以面向接口编程,而不是面向具体的实现类.

   ```java
   public class Airport {
       // 这个方法可以接受任何实现了 Flyable 接口的对象
       public void performFlight(Flyable flyingObject) {
           System.out.println("--- 飞行表演开始 ---");
           flyingObject.takeOff();
           flyingObject.fly();
           flyingObject.land();
           System.out.println("--- 飞行表演结束 ---\n");
       }
   }
   
   public class Main {
       public static void main(String[] args) {
           Airport airport = new Airport();
   
           Flyable bird = new Bird();
           Flyable airplane = new Airplane();
           Flyable kite = new Kite();
   
           airport.performFlight(bird);
   
           airport.performFlight(airplane);
   
           airport.performFlight(kite);
       }
   }
   ```

   `performFlight` 方法不关心传来的是鸟、飞机还是风筝,它只认 `Flyable` 这个接口.这大大提高了代码的扩展性.如果未来我们新增一个 `Drone`（无人机）类,只要它也实现 `Flyable` 接口,就可以无缝地被 `performFlight` 方法调用,无需修改任何现有代码.

2. **定义程序的契约和规范 (API Contract)** 在大型项目中,不同模块或团队之间可以通过接口来约定功能.例如,A 团队负责定义接口（需要什么功能）,B 团队负责实现这些接口（如何实现这些功能）,这实现了开发的解耦,双方可以并行工作.

3. **降低耦合度 (Decoupling)** 通过面向接口编程,模块之间的依赖关系从“依赖具体的类”转变为“依赖抽象的接口”.这样,只要接口不变,具体的实现类可以随意更换,而不会影响到调用方.这在**依赖注入 (Dependency Injection)** 和各种设计模式中是核心思想.





| 应用场景                  | 核心目的                                                     | 典型例子                                       |
| ------------------------- | ------------------------------------------------------------ | ---------------------------------------------- |
| **事件监听/回调**         | **解耦** 和 **异步通知**。让一个对象能在特定事件发生时，通知另一个关心该事件的对象 | `View.OnClickListener`, `TextWatcher`          |
| **网络/数据库API定义**    | **定义契约**。只声明需要做什么（如获取数据），而不关心具体怎么做（由库生成实现） | Retrofit 的` Service `接口, Room 的 `DAO` 接口 |
| **Fragment-Activity通信** | **解耦** 和 **标准化交互**。让 Fragment 可复用，不依赖于任何特定的 Activity | 自定义的回调接口                               |
| **依赖注入(DI)**          | **面向接口编程**。降低模块间依赖，方便替换实现和进行单元测试 | 在 Hilt/Dagger 中为` Repository `等定义接口    |





## 接口与抽象类的区别

| 特性         | 接口 (Interface)                        | 抽象类 (Abstract Class)                  |
| ------------ | --------------------------------------- | ---------------------------------------- |
| **设计意图** | 定义一种**能力**或**行为规范** (can-do) | 定义一个**族系**或**模板** (is-a)        |
| **关系**     | 类与接口是**实现**关系 (`implements`。  | 类与抽象类是**继承**关系 (`extends`)     |
| **继承限制** | 一个类可以**实现多个**接口              | 一个类**只能继承一个**父类（包括抽象类） |
| **主要用途** | 做功能的解耦合                          | 实现代码复用性                           |

**总结一下**：

- 当你想要定义一组不相关的类应该共同遵守的行为规范时，使用**接口**.例如，`Flyable`、`Runnable`（可运行的）、`Serializable`（可序列化的）.
- 当你想要为一组高度相关的类创建一个共享代码和通用特性的基类时,使用**抽象类**.例如，`Shape`（图形）是 `Circle`（圆形）和 `Square`（方形）的抽象父类.



---



# 代码块

## 实例代码块 

实例代码块,或称构造代码块,是直接定义在类中的、没有 `static` 关键字修饰的代码块.



```Java
public class MyClass {
    // 这是一个实例代码块
    {
        System.out.println("实例代码块执行了");
    }

    public MyClass() {
        System.out.println("构造方法执行了");
    }
}
```





*核心特性*

- **执行时机**: **每次创建类的实例（对象）时**执行.它的执行顺序在**父类构造方法之后,当前类构造方法之前**.
- **主要用途**: 用于初始化实例变量（非静态成员变量）,或者执行所有对象在创建时都需要运行的通用逻辑.它可以被看作是所有构造方法的“公共前缀”,有助于减少构造方法之间的代码重复.
- **访问权限**: 可以访问类的**任何成员**,包括实例成员（变量和方法）和静态成员（变量和方法）.因为它在对象创建时运行,所以此时实例成员已经分配了内存.



## 静态代码块

静态代码块是使用 `static` 关键字修饰的、直接定义在类中的代码块.



```java
public class MyClass {
    // 这是一个静态代码块
    static {
        System.out.println("静态代码块执行了");
    }
}
```



*核心特性*

- **执行时机**: 在 **类第一次被加载到 JVM（Java 虚拟机）时**执行.这通常发生在第一次创建该类的对象、第一次访问该类的静态成员、或通过反射加载该类时.
- **执行频率**: **整个程序的生命周期中只执行一次**.
- **主要用途**: 用于对类级别的静态成员（`static` 变量）进行复杂的初始化,或者执行一些类级别的、只需要进行一次的设置操作（如加载数据库驱动、初始化配置文件等）.
- **访问权限**: **只能访问类的静态成员（静态变量和静态方法）**.它**不能**直接访问实例成员（非静态变量和方法）,因为在静态代码块执行时,类的任何实例（对象）都还没有被创建,实例成员尚未分配内存.



------





| 特性         | 静态代码块 (Static Block)                                    | 实例代码块 (Instance Block)                                  |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **关键字**   | 使用 `static {}` 定义                                        | 直接使用 `{}` 定义                                           |
| **执行时机** | 类第一次加载到 JVM 时执行                                    | 每次创建类的实例（`new`一个对象）时执行                      |
| **执行频率** | **仅执行一次**                                               | 每创建一个对象就**执行一次**                                 |
| **执行顺序** | **最先执行**，在任何对象创建之前                             | 在构造方法之前执行                                           |
| **访问能力** | **只能**访问静态成员（变量和方法）                           | **可以**访问静态成员和实例成员                               |
| **主要用途** | 对静态变量进行初始化，或执行类级别的、一次性的准备工作（如加载驱动） | 对实例变量进行初始化，提取多个构造方法中的公共代码，执行对象级别的通用逻辑 |











```java
public class CodeBlockExample {

    // 静态变量
    private static String staticField = "初始静态字段";

    // 实例变量
    private String instanceField = "初始实例字段";

    // 静态代码块
    static {
        System.out.println("1. 静态代码块执行");
        System.out.println("   - 静态字段值: " + staticField);
        // System.out.println(instanceField); // 编译错误！不能访问实例字段
        staticField = "静态代码块修改后的静态字段";
        System.out.println("   - 修改后静态字段值: " + staticField);
    }

    // 实例代码块
    {
        System.out.println("2. 实例代码块执行");
        System.out.println("   - 静态字段值: " + staticField);
        System.out.println("   - 实例字段值: " + instanceField);
        this.instanceField = "实例代码块修改后的实例字段";
        System.out.println("   - 修改后实例字段值: " + this.instanceField);
    }

    // 构造方法
    public CodeBlockExample() {
        System.out.println("3. 构造方法执行");
        System.out.println("   - 实例字段值: " + this.instanceField);
    }

    public static void main(String[] args) {
        System.out.println("--- main 方法开始 ---");

        System.out.println("\n--- 准备创建第一个实例 ---");
        new CodeBlockExample();

        System.out.println("\n--- 准备创建第二个实例 ---");
        new CodeBlockExample();
        
        System.out.println("\n--- main 方法结束 ---");
    }
}
```





```
1. 静态代码块执行
   - 静态字段值: 初始静态字段
   - 修改后静态字段值: 静态代码块修改后的静态字段
--- main 方法开始 ---

--- 准备创建第一个实例 ---
2. 实例代码块执行
   - 静态字段值: 静态代码块修改后的静态字段
   - 实例字段值: 初始实例字段
   - 修改后实例字段值: 实例代码块修改后的实例字段
3. 构造方法执行
   - 实例字段值: 实例代码块修改后的实例字段

--- 准备创建第二个实例 ---
2. 实例代码块执行
   - 静态字段值: 静态代码块修改后的静态字段
   - 实例字段值: 初始实例字段
   - 修改后实例字段值: 实例代码块修改后的实例字段
3. 构造方法执行
   - 实例字段值: 实例代码块修改后的实例字段

--- main 方法结束 ---
```



1. **静态代码块只执行了一次**：在 `main` 方法开始执行,`CodeBlockExample` 类被加载时,静态代码块就立刻执行了,并且之后再也没有执行过.
2. **实例代码块和构造方法执行了两次**：每次调用 `new CodeBlockExample()` 创建新对象时,实例代码块和构造方法都会按顺序执行一次.
3. **执行顺序**：对于单个对象的创建过程,顺序是 **实例代码块 -> 构造方法**.
4. **访问权限**：静态代码块中无法访问 `instanceField`,而实例代码块中可以自由访问 `staticField` 和 `instanceField`.



---





# 内部类

*优点：*

1. **逻辑分组与封装**：如果一个类只被某一个其他类使用，那么将它定义为内部类可以从逻辑上将它们组织在一起。这隐藏了实现细节，使得代码结构更清晰。
2. **增强封装性**：内部类可以访问其外部类的所有成员，包括私有成员（private fields and methods）。这提供了一种特殊的访问权限，可以实现更紧密的协作，而无需将外部类成员声明为 `public` 或 `protected`。
3. **代码更简洁可读**：尤其是在事件处理（如 GUI 编程）或实现某些设计模式（如迭代器模式）时，使用内部类（特别是匿名内部类）可以让代码更紧凑、更贴近使用场景。





## 成员内部类



这是最常见的内部类形式，它像外部类的成员变量一样，定义在类的内部、方法的外部。

*特点：*

- **依赖外部类实例**：成员内部类的实例必须依附于一个外部类的实例,你不能在没有外部类对象的情况下创建成员内部类对象.
- **访问权限**：它可以无条件地访问外部类的所有成员（包括静态和非静态，私有和公有）.
- **不能有静态成员**：成员内部类中不能定义任何静态（`static`）方法或变量,除非是 `static final` 的常量.
- **`this` 关键字**：在内部类中,`this` 指向内部类自身的实例,要引用外部类的实例需要使用 `OuterClassName.this`.

```Java
public class Outer {
    private String outerField = "Outer Field";
    private static String staticOuterField = "Static Outer Field";

    // 成员内部类
    public class Inner {
        private String innerField = "Inner Field";

        public void display() {
            // 直接访问外部类的私有成员
            System.out.println("Accessing outer field: " + outerField);
            // 直接访问外部类的静态成员
            System.out.println("Accessing static outer field: " + staticOuterField);
            // 访问内部类自己的成员
            System.out.println("Accessing inner field: " + this.innerField);
            // 访问外部类的实例
            System.out.println("Accessing outer instance field using 'this': " + Outer.this.outerField);
        }
    }

    public static void main(String[] args) {
        // 1. 创建外部类实例
        Outer outer = new Outer();
        // 2. 通过外部类实例创建内部类实例
        Outer.Inner inner = outer.new Inner();
        
        inner.display();
    }
}
```

**如何实例化：** `OuterClass.InnerClass innerObject = outerObject.new InnerClass();`



## 静态内部类



静态内部类使用 `static` 关键字修饰.它与外部类的关系更像是“命名空间”的归属关系,而不是成员关系.

*特点：*

- **不依赖外部类实例**：创建静态内部类的实例不需要外部类的实例.
- **访问权限**：它只能访问外部类的静态（`static`）成员,不能直接访问外部类的非静态（实例）成员.
- **可以有静态成员**：静态内部类可以拥有自己的静态和非静态成员.
- **行为类似普通类**：除了定义的位置和对外部类静态成员的访问权限外,它几乎和一个普通的顶级类一样.



```Java
public class Outer {
    private String outerField = "Outer Field";
    private static String staticOuterField = "Static Outer Field";

    // 静态内部类
    public static class StaticInner {
        private String innerField = "Static Inner Field";

        public void display() {
            // 错误！不能直接访问外部类的非静态成员
            // System.out.println(outerField); 
            
            // 可以访问外部类的静态成员
            System.out.println("Accessing static outer field: " + staticOuterField);
            
            // 访问自己的成员
            System.out.println("Accessing inner field: " + innerField);
        }
    }

    public static void main(String[] args) {
        // 直接创建静态内部类的实例，无需外部类实例
        Outer.StaticInner staticInner = new Outer.StaticInner();
        
        staticInner.display();
    }
}
```

**如何实例化：** `OuterClass.StaticInnerClass nestedObject = new OuterClass.StaticInnerClass();`



##  局部内部类 



局部内部类是定义在一个方法或者一个作用域（如 `if` 语句块）内部的类.

*特点：*

- **作用域极小**：它的生命周期和可见性仅限于定义它的方法或块中.
- **不能有访问修饰符**：不能使用 `public`,`private` 等修饰符,也不能使用 `static` 修饰.
- **访问外部成员**：和成员内部类一样,它可以访问外部类的所有成员.
- **访问局部变量**：它可以访问所在方法中的局部变量,但这些变量必须是 `final` 或“事实上的 final”（effectively final,即变量在初始化后没有被再次赋值）.



```Java
public class Outer {
    private String outerField = "Outer Field";

    public void someMethod() {
        final String localVariable = "Local Variable"; // 必须是 final 或 effectively final

        // 局部内部类
        class LocalInner {
            public void display() {
                // 访问外部类成员
                System.out.println("Accessing outer field: " + outerField);
                // 访问方法内的局部变量
                System.out.println("Accessing local variable: " + localVariable);
            }
        }

        // 在方法内创建并使用局部内部类
        LocalInner localInner = new LocalInner();
        localInner.display();
    }

    public static void main(String[] args) {
        Outer outer = new Outer();
        outer.someMethod();
    }
}
```



## 匿名内部类 



匿名内部类是没有名字的局部内部类.它通常用于快速创建某个类或接口的子类实例,非常适合“一次性使用”的场景.



*特点：*

- **没有名字**：它在定义的同时就创建了实例.
- **语法**：通常是 `new SuperType() { ... }` 或 `new Interface() { ... }` 的形式.
- **没有构造函数**：因为它没有名字,所以不能定义构造函数,如果需要初始化,可以使用实例初始化块.
- **适用场景**：最常用于实现接口（如 `Runnable`, `Comparator`）或作为事件监听器.
- **访问规则**：和局部内部类一样,可以访问外部类的所有成员,以及所在方法中 `final` 或“事实上的 final”的局部变量.



```java
// 使用接口
interface Greeting {
    void sayHello();
}

public class Outer {
    public void startThread() {
        // 使用匿名内部类实现 Runnable 接口
        new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("A new thread is running...");
            }
        }).start();
    }

    public void greetSomeone() {
        // 使用匿名内部类实现 Greeting 接口
        Greeting englishGreeting = new Greeting() {
            @Override
            public void sayHello() {
                System.out.println("Hello!");
            }
        };
        englishGreeting.sayHello();
    }
    
    public static void main(String[] args) {
        Outer outer = new Outer();
        outer.startThread();
        outer.greetSomeone();
    }
}
```









| 特性                | 成员内部类                                                   | 静态内部类                                                   | 局部内部类                                                   | 匿名内部类                                                   |
| ------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **定义位置**        | 类的成员位置                                                 | 类的成员位置                                                 | 方法或代码块内                                               | 表达式中（new之后）                                          |
| **`static` 关键字** | 不能使用                                                     | **必须使用**                                                 | 不能使用                                                     | 不能使用                                                     |
| **访问外部类成员**  | 所有成员                                                     | **仅静态成员**                                               | 所有成员                                                     | 所有成员                                                     |
| **访问局部变量**    | N/A                                                          | N/A                                                          | 必须是`final`或`effectively final`                           | 必须是`final`或`effectively final`                           |
| **创建实例方式**    | `outer.new Inner()`                                          | `new Outer.Inner()`                                          | 在方法内 `new Inner()`                                       | `new Interface(){...}`                                       |
| **依赖外部实例**    | **是**                                                       | **否**                                                       | 是                                                           | 是                                                           |
| **拥有静态成员**    | 否 (除了`static final`)                                      | **是**                                                       | 否                                                           | 否                                                           |
| **应用场景**        | 创建与外部类实例状态紧密相关的辅助类,如实现迭代器（Iterator）. | 仅为逻辑上组织,与外部类实例无关,如 `Map.Entry` 或建造者模式（Builder Pattern）. | 在方法内需要一个临时的、具名的类,且实现比匿名类复杂时使用（较少用）. | 用于实现事件监听器、`Runnable`、`Comparator` 等只需一次性使用的简单接口或类。 |





---





# 函数式编程

**函数式编程**是一种编程范式,就像面向对象编程一样,它将计算机运算视为数学函数的求值,并避免使用**可变状态和副作用**.

简单来说,它的核心思想是：**像对待数学函数一样对待程序中的函数**。

我们来分解一下这个概念的关键特征：



*a. 函数是“一等公民”（First-Class Citizens）*

这是函数式编程最核心的特点。它意味着函数和其他数据类型（如 `int`、`String`、对象）处于同等地位.具体表现为：

- **可以作为变量赋值**：`Function<Integer, Integer> square = x -> x * x;`
- **可以作为参数传递**：将一个函数传递给另一个方法.
- **可以作为返回值返回**：一个方法可以返回一个函数.

Java 通过**函数式接口**和 **Lambda 表达式**实现了这一点.



*b. 纯函数（Pure Functions）*

纯函数是指满足以下两个条件的函数：

1. **无副作用**：函数不会修改其作用域之外的任何状态.例如,它不会修改全局变量、类的成员变量,也不会进行 I/O 操作（如打印到控制台、读写文件）.
2. **引用透明性：**对于相同的输入,永远返回相同的输出.函数的结果只依赖于其输入参数,而不依赖于任何外部状态.

```java
// 纯函数：结果只依赖于输入 a 和 b
public int sum(int a, int b) {
    return a + b;
}

// 非纯函数：因为它修改了外部状态 (total)
private int total = 0;
public int addToTotal(int a) {
    this.total += a; // 副作用：修改了成员变量
    return this.total;
}
```

纯函数的好处是巨大的：

- **可预测性**：给定输入，输出总是确定的，非常容易理解和推理。
- **易于测试**：不需要复杂的 Mock 或环境设置，只需提供输入并验证输出即可。
- **并行/并发安全**：因为纯函数不共享和修改状态，所以它们天然是线程安全的，可以安全地并行执行。



*c. 不可变性（Immutability）*

函数式编程推崇创建不可变的数据结构。一旦创建了数据，就不再修改它。如果需要修改，应该是创建一个包含新值的新对象，而不是在原地修改旧对象。

这避免了因状态变化而引起的复杂性和错误，尤其是在多线程环境中。Java 中的 `String` 类就是一个典型的不可变对象。



*d. 声明式编程（Declarative） vs. 命令式编程（Imperative）*

- **命令式编程**：关注**“如何做”（How）**.你需要一步步地告诉计算机执行的具体指令,传统的 `for` 循环就是典型的命令式编程.
- **声明式编程**：关注**“做什么”（What）**.你只描述你想要的结果,而不关心具体的实现步骤,函数式编程就是一种声明式编程.

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6);

// 命令式编程 (Imperative)
List<Integer> evenDoubled = new ArrayList<>();
for (Integer number : numbers) {
    if (number % 2 == 0) {
        evenDoubled.add(number * 2);
    }
}

// 声明式编程 (Declarative / Functional)
List<Integer> evenDoubledFunctional = numbers.stream()
                                             .filter(n -> n % 2 == 0) // 做什么：筛选偶数
                                             .map(n -> n * 2)         // 做什么：乘以 2
                                             .collect(Collectors.toList());
```



---



## Lambda 表达式

Lambda 表达式(闭包)是`java8`的新特性,Lambda运行将函数作为一个方法的参数,也就是函数作为参数传递到方法中,使用lambda表达式可以让代码更加简洁.

Lambda表达式的使用场景: **用以简化匿名内部类和接口实现.**

关于接口实现,可以有很多种方式来实现.例如: 设计接口的实现类,使用匿名内部类.但是lambda表达式,比这两种方式都简单.



### 函数式接口

虽然说,lambda表达式可以在⼀定程度上简化接口的实现.但是,并不是所有的接口都可以使用lambda表达式来简洁实现的.

lambda表达式毕竟只是⼀个匿名方法,当实现的接口中的方法过多或者多少的时候,lambda表达式都是不适用的.

**lambda表达式只能实现函数式接口。**

如果说,⼀个接口中，要求实现类必须实现的抽象方法,有且只有⼀个！这样的接口,就是函数式接口.

```java
//有且只有一个实现类必须要实现的抽象方法，所以是函数式接口
interface Test{
    public void test();
}
```



### `@FunctionalInterface`

是⼀个注解,用在接口之前,判断这个接口是否是⼀个函数式接口.如果是函数式接口,没有任何问题.如果不是函数式接口,则会报错.功能类似于 `@Override`.
```java
@FunctionalInterface
interface Test{
    public void test();
}
```





### Lambda 表达式的核心语法

Lambda 表达式的语法非常紧凑,其基本结构如下：

```java
(parameters) -> expression
```

或者

```java
(parameters) -> { statements; }
```

它由三个部分组成：

1. **参数列表 (parameters)**：方法的参数.
   - 如果没有参数,必须使用一对空括号 `()`.
   - 如果只有一个参数,可以省略括号.
   - 参数的类型通常可以由编译器根据上下文推断出来,所以大部分情况下可以省略.
2. **箭头操作符 (->)**：也称为 Lambda 操作符,它将参数列表和 Lambda 主体分开.
3. **Lambda 主体 (body)**：
   - 如果主体只有**一条语句**,可以省略大括号 `{}`和`return`.表达式的结果会自动作为返回值.
   - 如果主体包含**多条语句**,则必须使用大括号 `{}` 将它们包裹起来,并且如果方法需要返回值,必须显式使用 `return` 语句.

```java

        // --- 2. 未使用 Lambda (通过匿名内部类) ---
        // 整个结构非常冗长,核心逻辑只有一行 s.startsWith("B"),Predicate是函数式接口
        
        names.removeIf(new Predicate<String>() {
            @Override
            public boolean test(String s) {
                // 如果字符串以 "B" 开头，返回 true，该元素将被移除
                return s.startsWith("B");
            }
        });

        System.out.println("未使用 Lambda 过滤后: " + names);


        // --- 3. 使用 Lambda 表达式 ---
        // s -> s.startsWith("B") 就是对 Predicate 接口中 test 方法的实现。
        // s 是参数，-> 右边是方法体。
        
        names.removeIf(s -> s.startsWith("B"));

        System.out.println("使用 Lambda 过滤后: " + names);

```







---





## 方法引用

方法引用是 Java 8 中与 Lambda 表达式一同引入的一项重要特性,你可以把它看作是 Lambda 表达式的一种**语法糖**.当你想要使用的 Lambda 表达式的实现恰好是调用一个**已经存在的方法**时,可以用方法引用来进一步简化代码,使其更加简洁、清晰和可读.

方法引用使用 `::`（双冒号）操作符.`::` 的**左边是类名或对象名,右边是方法名或 `new` 关键字.**



**使用 Lambda 表达式：**



```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
names.forEach(s -> System.out.println(s));
```

观察这个 Lambda 表达式 `s -> System.out.println(s)`.它所做的唯一一件事就是：接收一个参数 `s`,然后立刻将这个 `s` 作为参数传递给 `System.out.println()` 方法.

在这种“**参数只是简单地传递给另一个方法**”的情况下,我们就可以使用方法引用来让代码变得更干净.

**使用方法引用：**



```Java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
names.forEach(System.out::println);
```

`System.out::println` 就是一个方法引用,它清晰地表达了“对列表中的每个元素,都调用 `System.out` 对象的 `println` 方法”这个意图.



### 静态方法引用 



- **语法**：`ClassName::staticMethodName`
- **说明**：如果 Lambda 表达式的主体只是调用一个静态方法,并且 Lambda 的参数列表与该静态方法的参数列表相匹配.

**示例：** 将一个字符串列表转换为整数列表,



```java
List<String> numbersAsStrings = Arrays.asList("1", "2", "3");

// 使用 Lambda 表达式
List<Integer> numbers = numbersAsStrings.stream()
                                        .map(s -> Integer.parseInt(s))
                                        .collect(Collectors.toList());

// 使用静态方法引用
List<Integer> numbersRef = numbersAsStrings.stream()
                                           .map(Integer::parseInt)
                                           .collect(Collectors.toList());

System.out.println(numbersRef); // 输出: [1, 2, 3]
```

这里的 `Integer::parseInt` 就等同于 `s -> Integer.parseInt(s)`。

------



###  特定对象的实例方法引用 



- **语法**：`instance::instanceMethodName`
- **说明**：如果 Lambda 表达式的主体只是调用一个**已经存在**的对象的实例方法,并且 Lambda 的参数与该实例方法的参数相匹配.

**示例：** 我们回到开头的打印例子.



```Java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

// `System.out` 是 PrintStream 类的一个已经存在的实例对象
PrintStream out = System.out;

// 使用 Lambda 表达式
names.forEach(s -> out.println(s));

// 使用实例方法引用
names.forEach(out::println);
```

这里的 `System.out::println` 就是 `out::println`，因为 `System.out` 是一个已经存在的对象。

------



### 特定类型的任意对象的实例方法引用



- **语法**：`ClassName::instanceMethodName`
- **说明**：这是最特殊的一种.当 Lambda 表达式的**第一个参数**是实例方法的调用者,并且**后续参数**（如果有的话）被传递给该实例方法时,可以使用这种引用.

**示例：** 将一个字符串列表全部转为大写.



```Java
List<String> names = Arrays.asList("alice", "bob", "charlie");

// 使用 Lambda 表达式
List<String> upperNames = names.stream()
                               .map(s -> s.toUpperCase())
                               .collect(Collectors.toList());

// 使用方法引用
List<String> upperNamesRef = names.stream()
                                  .map(String::toUpperCase)
                                  .collect(Collectors.toList());

System.out.println(upperNamesRef); // 输出: [ALICE, BOB, CHARLIE]
```

这里,`map` 方法接收的 Lambda `s -> s.toUpperCase()` 中,第一个参数 `s` 正好是 `toUpperCase()` 方法的调用者.因此,它可以被简化为 `String::toUpperCase`.

**另一个例子：** 排序



```Java
String[] names = {"Charlie", "Alice", "Bob"};
// Lambda: (s1, s2) -> s1.compareToIgnoreCase(s2)
Arrays.sort(names, String::compareToIgnoreCase);
```

这里,第一个参数 `s1` 是方法的调用者,第二个参数 `s2` 是方法的参数,完美匹配.

------



### 构造方法引用 



- **语法**：`ClassName::new`
- **说明**：如果 Lambda 表达式的主体只是创建一个新对象,那么可以使用构造方法引用.

**示例：** 将一个字符串列表的内容复制到一个新的 `ArrayList` 中.



```Java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

// 使用 Lambda 表达式
Supplier<List<String>> listSupplier = () -> new ArrayList<>();
List<String> newNames = new ArrayList<>(names);

// 使用构造方法引用 (常用于 Stream 的 collect 操作)
List<String> copiedNames = names.stream()
                                .collect(Collectors.toCollection(ArrayList::new));

System.out.println(copiedNames);
```

这里的 `ArrayList::new` 就等同于 `() -> new ArrayList<>()`，它提供了一个创建 `ArrayList` 实例的功能。







- 方法引用是 Lambda 表达式的简化写法，让代码更具可读性。
- 当你发现你的 Lambda 表达式 `(...) -> ClassName.someMethod(...)` 或 `(...) -> object.someMethod(...)` 只是在简单地调用一个已存在的方法时，就应该考虑使用方法引用。
- 它和 Lambda 表达式一样，都必须在需要**函数式接口**的上下文中使用。编译器会根据函数式接口的抽象方法签名来推断并匹配对应的方法引用。





---





























