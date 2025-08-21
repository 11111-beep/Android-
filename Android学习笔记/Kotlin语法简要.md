# Kotlin语法简要

## 1.变量

**val**:不可变的变量

**var**:可变的变量

```kotlin
fun main(){
    val a=10
    // var a: Int=10
    println("a=$a")
}
```

---



## 2.函数

```kotlin
fun largerNumber(num1: Int, num2: Int) = max(num1, num2)
// 只有一行代码时可以不写函数体,类型推导机制让我们不必声明返回类型
```

---



## 3.函数的执行语句

### 🔹 if语句

if具有返回值,返回值就是if语句中最后一行代码的返回值

```kotlin
fun largerNumber(num1: Int, num2: Int) = if (num1>num2)num1 else num2
```

### 🔹 when条件语句(类似于switch)

和if一样也是自带返回值

```kotlin
fun getScore(name: String) = when (name) {
    "Tom" -> 86
    "Jim" -> 95
    "Lily" -> 100
    else -> 0
}
```

```kotlin
fun main() {
    val num = 10L // Long类型
    checkNumber(num)
}
fun checkNumber(num: Number) {
    when (num) {
        is Int -> println("number is Int")
        is Double -> println("number is Double")
        else -> println("number not support")
    }
}
// Number类型的参数是一个抽象类
```

```kotlin
fun getScore(name: String) = when {
    name.startsWith("Tom") -> 86 // 以Tom开头的名字都是86
    name == "Jim" -> 77
    name == "Jack" -> 95
    name == "Lily" -> 100
    else -> 0
}
```

### 循环语句

```kotlin
val range = 0..10 // [0,10]的区间
fun main(){
    for (i in 0..10){
        println(i)
    }
}
```

```kotlin
val range = 0 until 10 // [0,10)的区间
fun main(){
    for (i in 0 until 10 step 2){ // 每次递增2
        println(i)
    }
}
```

```kotlin
fun main(){
    for (i in 10 downTo 1){
        println(i) // 降序
    }
}
```

---



## 3.类与对象

```kotlin
class Person(var name: String, var age: Int) {
    var name: String = ""
    var age=0
    fun eat(){
        println("$name is eating. He is $age years old.")
    }
}
```

```kotlin
fun main(){
    val p = Person()
    P.name = "Jack"
    P.age = 19
    P.eat()
}
```

---



## 4.继承与构造函数

在Kotlin中任何一个非抽象类默认都是不可被继承的

```kotlin
open class Person(var name: String, var age: Int) { // 加上open就可以被继承
    var name: String = ""
    var age=0
    fun eat(){
        println(name+" is eating. He is "+ age +" years old.")
    }
}
```

```kotlin
class Student : Person() { // 继承用冒号
    var sno = ""
    var grade = 0                  sad /.
}
```

主构造函数

```kotlin
class Student(name: String, age: Int) : Person(name, age){
    init { // 创建实例后会执行该逻辑
        println("sno is $sno")
    }
}
```

---



## 5.接口

```kotlin
interface Study {
    fun readBooks()
    fun doHomeWork(){
        println("do homework default implementation.")
    }
}
```

```kotlin
class Student(name: String, age: Int) : Person(name, age), Study{ // 调用接口用逗号
    override fun readBooks() {
        println(name+" is reading.")
    }
    override fun doHomeWork() {
        println(name+" is doing homework.")
    }
}
```

---



## 5.数据类

```kotlin
data class Cellphone(val brand: String, var price: Double) // 数据类用data关键字
```

```kotlin
fun main(){
    val cellphone1 = Cellphone("Samsung", 1299.99)
    val cellphone2 = Cellphone("Samsung", 1299.99)
    println(cellphone1)
    println("&{cellphone1 == cellphone2}")
}
```

---



## 6.单例类(在全局只有一个实例)

```kotlin
object Singleton { // 单例类用odject关键字
    fun singletonTest() {
        println("SingletonTest is called.")
    }
}
```

```kotlin
Singleton.singletonTest() // 调用
```

---



## 7.Lambda编程



### 🔹常见使用场景总览

| 场景         | 描述                          | 示例                                        |
| ------------ | ----------------------------- | ------------------------------------------- |
| 1️⃣ 集合操作   | `map`、`filter`、`forEach` 等 | `list.filter { it.isNotEmpty() }`           |
| 2️⃣ 事件监听   | 点击、输入等                  | `button.setOnClickListener { showToast() }` |
| 3️⃣ 函数参数   | 高阶函数传入行为              | `repeat(5) { println("Hello") }`            |
| 4️⃣ 线程/协程  | 简化异步任务                  | `Thread { doWork() }.start()`               |
| 5️⃣ 自定义回调 | 封装逻辑，暴露接口            | `fetchData { result -> println(result) }`   |

### 🔹 集合的创建与遍历

**List**

```kotlin
val list = ListOf("Apple", "Banana", "Orange", "Pear", "Grape") // 不可变集合
for (fruit in list) {
    println(fruit)
}
```

```kotlin
val list = mutableListOf("Apple", "Banana", "Orange", "Pear", "Grape") // 可变集合
    list.add("Watermalon")
    val anyResult=list.any{ it.length <= 5}
    val allResult=list.all{ it.length <= 5}
    println("anyResult is $anyResult,allResult is $allResult")
    for (fruit in list) {
        println(fruit)
    }
```

**Set**

```kotlin
val list = SetOf("Apple", "Banana", "Orange", "Pear", "Grape") // 不包含重复元素 
for (fruit in list) {
    println(fruit)
}
```

**Map**

```kotlin
val map = mapOf("Apple" to 1, "Banana" to 2, "Orange" to 3,"Pear" to 4, "Grape" to 5)
    for ((fruit, number) in map) {
        println("fruit is $fruit, number is $number")
    }
```

### 🔹 集合的函数API

Lambda表达式的语法结构 : {参数名1: 参数类型, 参数名2: 参数类型 -> 函数体}

```kotlin
// 1. 定义一个具名 lambda 变量，然后传入 maxBy 函数
val lambda = { fruit: String -> fruit.length }
// 此处将 lambda 表达式赋值给变量 lambda，该表达式接收一个 String，返回该字符串的长度。
val maxLengthFruit1 = list.maxBy(lambda)
// 通过传入预先定义的 lambda，找出列表中字符串长度最大的元素。

// 2. 直接在 maxBy 函数调用中传入 lambda 表达式（括号内写法）
val maxLengthFruit2 = list.maxBy({ fruit: String -> fruit.length })
// 在括号内直接定义 lambda，显示指定参数类型为 String，然后返回 fruit.length。

// 3. 使用尾随 lambda 的写法（lambda 表达式放在圆括号外，但仍写出参数类型）
val maxLengthFruit3 = list.maxBy() { fruit: String -> fruit.length }
// 当函数的最后一个参数是 lambda 时，可以把它放在括号外；这里仍然明确了参数类型。

// 4. 更简洁的尾随 lambda 写法，省略了圆括号
val maxLengthFruit4 = list.maxBy { fruit: String -> fruit.length }
// 同上，使用尾随 lambda 并省略空的括号，lambda 内部仍然指定参数类型。

// 5. 省略 lambda 参数的类型，由编译器自动推断
val maxLengthFruit5 = list.maxBy { fruit -> fruit.length }
// 此写法中省略了参数类型，编译器根据上下文知道 fruit 是 String，因此代码更简洁。

// 6. 当 lambda 只有一个参数时，可以使用隐式参数 it
val maxLengthFruit6 = list.maxBy { it.length }
// 当 lambda 只有一个参数时，Kotlin 允许使用 it 作为默认参数名称，使代码更简洁。

```

map函数是最常用的一种函数API

```kotlin
fun main() {
    val list = listOf("Apple", "Banana", "Orange", "Pear", "Grape")
    val newList = list.map { it.toUpperCase() } // 将所有水果名变成大写模式
    for (fruit in newlist) {
    println(fruit)
   }
}
```

filter函数用于过滤集合中的数据

```kotlin
fun main() {
    val list = listOf("Apple", "Banana", "Orange", "Pear", "Grape")
    val newList = list.map { it.length <= 5 }.map { it.toUpperCase() } // 保留五个字母以内的水果
    for (fruit in newlist) {
    println(fruit)
   }
}
```

any和all函数

```kotlin
val list = mutableListOf("Apple", "Banana", "Orange", "Pear", "Grape")
    list.add("Watermalon")
    val anyResult=list.any{ it.length <= 5} // 只要一个满足条件
    val allResult=list.all{ it.length <= 5} // 所有的都要满足条件
    println("anyResult is $anyResult,allResult is $allResult")
```

### 🔹 Java函数式API的使用

```kotlin
Thread{
        println("Hello World")
    }.start()
```

---



## 8.空指针检查

Kotlin默认所有参数和变量不能为空

**?         ?.       ?: **

```kotlin
fun doStudy(study: Study?) { // 加上?可为空
    // 检查study是否不为null，避免空指针异常
    if (study != null) {
        study.readBooks()
        study.doHomeWork()
    }
    // 如果study为null，函数将不会执行任何操作
}

```

```kotlin
fun doStudy(study: Study?) { 
    study?.readBooks() //不为空就调用
    study?.doHomeWork()
}

```

```kotlin
val c = a ?: b // 左边为空就返回右边的结果
```

```kotlin
fun fetTextLength(text: String?) = text?.length ?: 0
```

**非空断言工具 : !!**

```kotlin
val upperCase = content!!.toUpperCase() // 不用空指针检查
```

**let函数**

```kotlin
fun doStudy(study: Study?) { 
    study?.let { stu ->
    stu.readBooks() //不为空就调用
    stu.doHomeWork()
   }
}

```

---



## 9.函数的参数默认值

```kotlin
fun printParams(num: Int = 100, str: String){ // 不传参数时返回默认值
    println("num is $num,str is $str")
}
fun main() {
    printParams(str = "world")
}
```

---



## 10.标准函数with,run和apply

### 🔹 with

- **作用**：
   用于对传入的对象执行一系列操作，省去重复引用该对象的麻烦。
   它并不是扩展函数，而是一个顶层函数。

- **返回值**：
   返回 lambda 表达式中最后一行代码的执行结果。

- **语法**：

  ```kotlin
  with(receiver) {
      // 在这里可以直接访问 receiver 的成员（属性和方法）
      // 最后一行的表达式将作为返回值
  }
  ```

- **示例代码**：

  ```kotlin
  // 假设有一个 Person 类
  data class Person(var name: String, var age: Int)
  
  fun main() {
      val person = Person("Alice", 25)
      // 使用 with 对 person 对象进行操作
      val info = with(person) {
          // 可以直接访问 person 的属性和方法
          println("Name: $name")
          println("Age: $age")
          // 返回一个结果字符串
          "$name is $age years old."
      }
      println(info) // 输出: "Alice is 25 years old."
  }
  ```



### 🔹 run

- **作用**：
   run 既可以作为扩展函数，也可以作为普通函数。作为扩展函数时，它允许在对象上直接调用，从而在 lambda 表达式内使用该对象的上下文。

- **返回值**：
   返回 lambda 表达式中最后一行代码的执行结果。

- **语法**：

  ```kotlin
  receiver.run {
      // 在 lambda 中，this 指向 receiver，可以直接访问其成员
      // 最后一行的表达式的结果将作为返回值
  }
  ```

- **示例代码**：

  ```kotlin
  // 继续使用 Person 类
  fun main() {
      val person = Person("Alice", 25)
      // 使用 run 在 person 对象上直接调用
      val result = person.run {
          // 在这里可以直接引用 person 的属性和方法
          println("Name: $name")
          println("Age: $age")
          // 返回一个字符串作为结果
          "$name, age $age"
      }
      println(result) // 输出: "Alice, age 25"
  }
  ```



### 🔹 apply

- **作用**：
   apply 主要用于对象的初始化或配置。它以扩展函数的形式存在，可以对对象的属性进行设置，而无需多次引用对象本身。

- **返回值**：
   返回调用对象本身，而不是 lambda 中最后一行表达式的结果。

- **语法**:

  ```kotlin
  receiver.apply {
      // 在 lambda 中，this 指向 receiver
      // 对对象进行一系列配置或初始化操作
  }
  ```

- **示例代码**：

  ```kotlin
  // 继续使用 Person 类
  fun main() {
      // 使用 apply 对 Person 对象进行初始化
      val newPerson = Person("Bob", 30).apply {
          // 这里可以设置对象属性或者调用对象方法
          println("Initializing person: $name, $age")
          // 可以进行链式调用
          age += 1  // 修改属性
      }
      // apply 返回的是修改后的 newPerson 对象
      println(newPerson) // 输出: Person(name=Bob, age=31)
  }
  ```



### 🔹 总结

- **with**：
  - 需要传入对象作为参数。
  - 在 lambda 中操作对象，返回值为最后一行表达式的结果。
  - 适合对对象进行一系列操作后需要返回计算结果的场景。
- **run**：
  - 作为扩展函数使用，可在对象上直接调用。
  - 同样返回 lambda 的最后一行表达式结果。
  - 常用于执行一段代码块并返回一个计算结果。
- **apply**：
  - 主要用于对象的初始化与配置。
  - 返回调用对象本身，便于链式调用。
  - 适合在创建对象后需要对对象属性进行设置的场景。

---



## 11.定义静态方法(不需要实例就可以调用的方法)

### 🔹 运用注解

```kotlin
class Util {
    fun doAction1() {
        println("do action1")
    }
    
    companion object {
        @JvmStatic
        fun doAction2() { // 静态方法
            println("do action2")
        }
    }
}
```

```kotlin
Util.doAction2() // 调用静态方法
```



### 🔹 运用顶层方法

创建一个Kotlin文件(File),在里面写上静态方法,后面可以直接调用

---



## 12.变量延迟初始化

使其不用进行判空处理,但注意要记得初始化

``` kotlin
// private var adapter: MsgAdapter?=null
private lateinit var adapter

if(!::adapter.isInitialized){
    // 初始化操作
}
```

---



## 13.使用密封类优化代码

选择时可以不加 else

~~~kotlin
/* interface Result
class Success(val msg: String) : Result
class Failure(val error: Exception) : Result */

sealed class Rexult
class Success(val msg: String) : Result()
class Failure(val error: Exception) : Result() 

~~~

---



## 14.扩展函数

扩展函数是 Kotlin 中的一种语言特性，它允许你在不继承或修改原有类源码的情况下，为已有的类添加新的功能。



**语法结构**

```kotlin
fun ClassName.methodName(param1: Int, param2: Int): Int {
    return 0;
}
```



**创建一个String.kt 文件**

```kotlin
fun String.lettersCount(): int {
    var count = 0
    for (char in this) {
        if (char.isLetter()){
            count++
        }
    }
    return count
}
```

```kotlin
val count = "AVSVGS_+".lettersCount()
```

---



## 15.运算符重载

运算符重载允许你为自定义类型定义标准运算符（如 +、-、*、/ 等）的行为，从而使得这些类型的对象可以像基本数据类型一样使用运算符。运算符重载通常通过定义特定名称的函数来实现，在 Kotlin 中需要使用 `operator` 修饰符。例如，为了重载加号运算符，你可以为一个类定义一个 `plus` 函数：

```kotlin
class Money(val value: Int) {
    operator fun plus(money: Money): Money {
        val sum = value + money.value
        return Money(sum)
    }
    operator fun plus(newValue: Int): Money {
        val sum = value + newValue
        return Money(sum)
    }
}
```

```kotlin
val money1 = Money(5)
val money2 = Money(10)
val money3 = Money1+money2
val money4 = money3 +20
println(money4.value)
```

---



## 16.高阶函数

在 Kotlin 中，高阶函数（Higher-Order Function）是指接受函数作为参数或者将函数作为返回值的函数。由于 Kotlin 将函数视为一等公民（first-class citizens），这使得你可以灵活地处理函数，实现更高的抽象和代码复用。下面详细介绍 Kotlin 中高阶函数的相关概念和用法。

### 🔹 接受函数作为参数

定义一个高阶函数，该函数接受一个函数作为参数：

```kotlin
// 定义一个高阶函数，接受两个整数和一个函数作为参数
fun calculate(a: Int, b: Int, operation: (Int, Int) -> Int): Int {
    return operation(a, b)
}

// 调用 calculate，并传入一个 lambda 表达式
fun main() {
    val sum = calculate(3, 4) { x, y -> x + y }
    println("Sum: $sum")  // 输出: Sum: 7
}
```

在这个例子中，`calculate` 函数通过参数 `operation` 接受了一个 lambda 表达式，用于定义具体的计算操作。

### 🔹 将函数作为返回值

一个函数可以返回另一个函数，例如：

```kotlin
// 定义一个函数，返回一个加法函数
fun getAdder(): (Int, Int) -> Int {
    return { x, y -> x + y }
}

fun main() {
    val adder = getAdder()
    println("Adder: ${adder(5, 10)}")  // 输出: Adder: 15
}
```

这里 `getAdder` 返回一个 lambda 表达式，该表达式实现了两个整数相加的功能。

---



## 17.内联函数

在 Kotlin 中，使用 `inline` 关键字声明的函数称为内联函数。编译器在编译过程中会将内联函数的代码直接插入到调用处，而不是生成独立的函数调用。这样做可以避免高阶函数传递 lambda 表达式时产生的对象分配和调用开销。

```kotlin
// 定义一个内联函数，接受两个整数和一个 lambda 表达式作为操作
inline fun operate(a: Int, b: Int, operation: (Int, Int) -> Int): Int {
    return operation(a, b)
}

fun main() {
    // 调用内联函数，并传入一个 lambda 表达式
    val sum = operate(3, 4) { x, y -> x + y }
    println("Sum: $sum")  // 输出: Sum: 7
}
```



### 🔹 noinline

**作用与场景**

- **默认行为**：在内联函数中，所有的 lambda 参数默认都会被内联（即直接替换到调用处），从而减少额外的函数调用开销。
- **使用场景**：有时需要将某个 lambda 参数当作普通对象传递，比如存储到变量中、放入集合、或者传递给其它非内联函数等，这时就不能将它内联，否则会破坏其“对象”特性。
- **noinline 关键字**：标记在 lambda 参数前，表示该参数在调用过程中**不内联**，编译器不会将该 lambda 展开到调用处。

```kotlin
// 定义一个内联函数，其中 block1 会内联，block2 不会内联
inline fun process(
    block1: () -> Unit,
    noinline block2: () -> Unit
) {
    block1()  // 会被内联展开
    block2()  // 不会被内联展开，可以作为对象使用
}
```



### 🔹 crossinline

**作用与场景**

- **非局部返回问题**：在内联函数中，lambda 参数默认允许使用非局部返回（non-local return），即在 lambda 中使用 `return` 可以直接返回到外层调用函数。但当 lambda 被传递到其他执行环境中（比如异步回调或其它嵌套调用）时，非局部返回可能会导致逻辑混乱或无法预料的行为。
- **使用场景**：如果希望 lambda 参数仍然内联（从而获得性能优势），但又不允许其使用非局部返回，就可以使用 crossinline。
- **crossinline 关键字**：标记在 lambda 参数前，强制该 lambda 内的 `return` 只能用于返回 lambda 本身，不能用于返回外层函数。

```kotlin
// 定义一个内联函数，其中 block 参数被标记为 crossinline
inline fun execute(crossinline block: () -> Unit) {
    // 此处可能将 block 传递给另一个函数或在另一个上下文中执行
    val runnable = Runnable { 
        // 在这里使用 return 只能退出 lambda，而不能退出 execute 函数
        block()  
    }
    runnable.run()
}
```

---



## 18.高阶函数的运用

**简化SharedPreferences**

```kotlin
getSharedPreferences("data",Context.MODE_PRIVATE).edit {
    putString("name", "Tom")
    putInt("age", 28)
    putBoolean("married", false)
}
```



**简化ContentValues**

```kotlin
val values =contentValuesOf(
    "name" to "Game of Thrones",
    "authpr" to "George Martin",
    "pages" to 720,
    "price" to 20.85 
)
db.insert("Book",null,values)
```

---



## 19.泛型的基本用法

泛型允许我们在不指定具体类型的情况下进行编程

**定义泛型类**

```kotlin
class MyClass<T> {
    fun method(param: T): T {
        return param
    }
}
```

**调用泛型类**

```kotlin
val myClass = MyClass<Int>()
val result = myClass.method(123)
```

**定义一个泛型方法**

 ```kotlin
 class MyClass {
     fun <T> method(param: T): T {
         return param
     }
 }
 ```

**调用一个泛型方法**

```kotlin
val myClass = MyClass()
val result = myClass.method(123)
```

**对泛型进行类型限制**

```kotlin
class MyClass {
    fun <T : Number> method(param: T): T {
        return param
    }
}
```

---



## 20.类 

**概念：**

类委托是 Kotlin 提供的一种设计模式，可以将接口的实现委托给另一个对象。也就是说，当一个类需要实现某个接口时，可以将接口中的部分或全部方法调用转交给另一个对象处理，从而实现代码复用和职责分离。

**用途：**

- 简化代码实现，避免重复编写接口方法。
- 通过组合而非继承来扩展类的功能。
- 动态改变行为，增强灵活性。

```kotlin
// 定义一个接口
interface Base {
    fun printMessage()
}

// 实现接口的类
class BaseImpl(val message: String) : Base {
    override fun printMessage() {
        println(message)
    }
}

// 使用类委托的方式实现接口
class Derived(b: Base) : Base by b

fun main() {
    val baseImpl = BaseImpl("Hello, Kotlin Delegation!")
    val derived = Derived(baseImpl)
    derived.printMessage()  // 输出：Hello, Kotlin Delegation!
}
```

在上面的例子中，`Derived` 类通过 `: Base by b` 将 `Base` 接口的实现委托给了 `baseImpl` 对象，因此调用 `derived.printMessage()` 实际上调用的是 `baseImpl` 中的方法实现。

---



## 21.委托属性

**概念：**
 委托属性允许你将属性的 getter 和 setter 的逻辑交由另一个对象处理，这个对象称为“委托”。Kotlin 标准库提供了一些常用的委托，如 `lazy`、`observable`、`vetoable` 等，也可以自定义委托。

**用途：**

- 实现延迟初始化（如 `lazy` 委托）。
- 属性变更监听（如 `observable` 委托）。
- 属性值验证、缓存等其他特殊逻辑。
- 简化重复代码，提高代码复用性。

1. **延迟初始化（Lazy Delegation）**

   ```kotlin
   val lazyValue: String by lazy {
       println("Computed!")
       "Hello, Lazy Delegation!"
   }
   
   fun main() {
       println("Before accessing lazyValue")
       println(lazyValue)  // 第一次访问时，计算并打印 "Computed!" 和 "Hello, Lazy Delegation!"
       println(lazyValue)  // 后续访问直接返回已计算的值
   }
   ```

2. **可观察属性（Observable Delegation）**

   ```kotlin
   var observedValue: Int by Delegates.observable(0) { property, oldValue, newValue ->
       println("属性 ${property.name} 从 $oldValue 变为 $newValue")
   }
   
   fun main() {
       observedValue = 10  // 输出：属性 observedValue 从 0 变为 10
       observedValue = 20  // 输出：属性 observedValue 从 10 变为 20
   }
   ```

在这两个例子中，委托属性分别将属性初始化逻辑和修改监听逻辑分离到专门的委托对象中，从而让属性本身的声明更加简洁。



## 22.infix函数构造更可读的语法(改名字)

创建一个`beginsWith`函数,使其具有`startsWith`函数的功能

```kotlin
infix fun String.beginsWith(prefix: String) = startsWith(prefix)
```

创建一个`has`函数,使其具有`contains`函数的功能

```kotlin
infix fun <T> Collection<T>.has(element: T) = contains(element)
```

---



## 23.对泛型进行实化

在 **Kotlin 的内联函数（inline function）** 中，我们可以使用 `reified` 关键字**实化泛型**，使得泛型参数在**运行时仍然可以被访问**。

### **🔹 使用 `reified` 实化泛型**

```kotlin
inline fun <reified T> printType() {
    println("类型是：${T::class.simpleName}")
}

printType<Int>()   // 输出: 类型是：Int
printType<String>() // 输出: 类型是：String
```

**泛型 `T` 仍然存在于运行时**，可以用 `T::class.simpleName` 获取类型名称。

------

### **🔹 为什么要使用 `reified`？**

#### **(1) 运行时获取泛型类型**

```kotlin
inline fun <reified T> isType(value: Any): Boolean {
    return value is T  // 运行时可以判断类型
}

println(isType<String>("Hello")) // 输出: true
println(isType<Int>("Hello"))    // 输出: false
```

 **普通泛型无法做到 `is T` 判断，而 `reified` 泛型可以！**

------

#### **(2) 在泛型函数中创建对象**

使用 `reified` 可以直接创建对象，而普通泛型无法做到：

```kotlin
inline fun <reified T> createInstance(): T? {
    return T::class.java.getDeclaredConstructor().newInstance()
}

data class User(val name: String = "Default")

val user: User? = createInstance<User>()
println(user) // 输出: User(name=Default)
```

**可以直接实例化泛型类型 `T`，避免手动传递 `Class<T>`。**

------

#### **(3) 简化 `Class` 作为参数的 API**

在 Java 中，我们通常需要显式传递 `Class<T>`：

```kotlin
public <T> T fromJson(String json, Class<T> clazz) {
    return new Gson().fromJson(json, clazz);
}
```

在 Kotlin 中，`reified` 让 `Class<T>` 变得**不再必要**：

```kotlin
inline fun <reified T> Gson.fromJson(json: String): T {
    return fromJson(json, T::class.java)
}

val json = """{"name":"Alice"}"""
val user: User = Gson().fromJson<User>(json)
println(user)  // 输出: User(name=Alice)
```

**可以直接使用 `T::class.java`，无需手动传递 `Class<T>`！**

---

#### **(4) 简化 `Activity` 的启动**

```kotlin
inline fun <reified T> startActivity(context: Context, block: Intent.() -> Unit) {
    val intent = Intent(context, T::class,java)
    intent.block()
    context.startActivity(intent)
}
```

```kotlin
startActivity<TestActivity>(context) {
    putExtra("param1", "data")
    putExtra("param2", 123)
}
```

---



## 24.**泛型的协变与逆变**

在 Kotlin 中，泛型可以是 **协变（covariant）** 或 **逆变（contravariant）**，用于控制泛型类型参数在继承关系中的行为，以保证类型安全。



### **🔹 什么是协变？**

**协变允许子类型泛型对象赋值给父类型泛型对象**，但泛型类型参数**只能作为返回值，不能作为参数**。

**规则：`Producer<T>`（生产者）—— `T` 只能被返回，不可被消费**

- **关键字：`out`**
- **适用于只“生产”数据的场景**
- **子类泛型对象可以赋值给父类泛型对象**

### **🔹 举例**

假设我们有一个 `Animal` 类和 `Dog` 类：

```kotlin
open class Animal
class Dog : Animal()
```

定义一个协变接口：

```kotlin
interface Producer<out T> {  // 使用 `out` 关键字
    fun produce(): T  // 只能返回 T，不允许接收 T
}
```

 **可以将 `Producer<Dog>` 赋值给 `Producer<Animal>`**

```kotlin
class DogProducer : Producer<Dog> {
    override fun produce(): Dog {
        return Dog()
    }
}

val dogProducer: Producer<Dog> = DogProducer()
val animalProducer: Producer<Animal> = dogProducer  // 允许赋值
```

 **为什么可以赋值？**

- `Producer<Dog>` 只能 **返回** `Dog`，而 `Producer<Animal>` 也应该能返回 `Animal`
- 由于 `Dog` 是 `Animal` 的子类，因此 `Dog` 的生产者也可以充当 `Animal` 的生产者

 **错误：协变泛型不能用作输入参数**

```kotlin
interface Producer<out T> {
    fun produce(): T
    fun consume(value: T)  // 报错：out 泛型不能用于输入参数
}
```

### **🔹 适用场景**

- **生产者模式（Producer）**，例如：
  - `List<T>`（`List<Dog>` 可以赋值给 `List<Animal>`）
  - `Sequence<T>`

### **🔹 什么是逆变？**

**逆变允许父类型泛型对象赋值给子类型泛型对象**，但泛型类型参数**只能作为输入参数，不能作为返回值**。

**规则：`Consumer<T>`（消费者）—— `T` 只能被消费，不可被返回**

- **关键字：`in`**
- **适用于只“消费”数据的场景**
- **父类泛型对象可以赋值给子类泛型对象**

### **🔹 举例**

定义一个逆变接口：

```kotlin
interface Consumer<in T> {  // 使用 `in` 关键字
    fun consume(value: T)  // 只能接收 T，不允许返回 T
}
```

 **可以将 `Consumer<Animal>` 赋值给 `Consumer<Dog>`**

```kotlin
class AnimalConsumer : Consumer<Animal> {
    override fun consume(value: Animal) {
        println("Consuming an animal")
    }
}

val animalConsumer: Consumer<Animal> = AnimalConsumer()
val dogConsumer: Consumer<Dog> = animalConsumer  // 允许赋值
```

**为什么可以赋值？**

- `Consumer<Animal>` 需要消费 `Animal` 或其子类
- `Consumer<Dog>` 也需要消费 `Dog`
- 由于 `Dog` 是 `Animal` 的子类，因此 `Consumer<Animal>` 也可以用于 `Consumer<Dog>`

**错误：逆变泛型不能用作返回值**

```kotlin
interface Consumer<in T> {
    fun consume(value: T)
    fun produce(): T  // 报错：in 泛型不能用于返回值
}
```

### **🔹 适用场景**

- **消费者模式（Consumer）**，例如：
  - `Comparator<T>`（比较器）
  - `Function<T, R>`（输入 `T`）

## 

| **对比项**             | **协变 (`out`)**                              | **逆变 (`in`)**                               |
| ---------------------- | --------------------------------------------- | --------------------------------------------- |
| **关键字**             | `out`                                         | `in`                                          |
| **可以作为返回值？**   | 可以                                          | 不可以                                        |
| **可以作为输入参数？** | 不可以                                        | 可以                                          |
| **泛型类型关系**       | `Producer<Dog>` 可以赋值给 `Producer<Animal>` | `Consumer<Animal>` 可以赋值给 `Consumer<Dog>` |
| **适用场景**           | 生产者（`Producer<T>`）                       | 消费者（`Consumer<T>`）                       |

| **特性**               | **协变（`out`）**                             | **逆变（`in`）**                              |
| ---------------------- | --------------------------------------------- | --------------------------------------------- |
| **作用**               | 生产者，返回 `T`                              | 消费者，接收 `T`                              |
| **关键字**             | `out`                                         | `in`                                          |
| **可以作为返回值？**   | 可以                                          | 不可以                                        |
| **可以作为输入参数？** | 不可以                                        | 可以                                          |
| **子类型赋值关系**     | `Producer<Dog>` 可以赋值给 `Producer<Animal>` | `Consumer<Animal>` 可以赋值给 `Consumer<Dog>` |
| **使用场景**           | `List<T>`、`Sequence<T>`                      | `Comparator<T>`、`Function<T, R>`             |

 **记住：**

- **“生产者用 `out`，消费者用 `in`”**
- **`List<T>` 是 `out`，`Comparator<T>` 是 `in`**
- **`MutableList<T>` 不能用 `out` 或 `in`**



## 25.协程

Kotlin 协程（Coroutines）是 Kotlin 提供的一种用于简化异步编程和并发编程的工具。它能够高效地管理多任务，并且与线程相比，协程的开销小、运行速度快。协程让我们能够以一种顺序的方式编写异步代码，避免了回调地狱、线程阻塞等问题。

### **🔹 什么是协程**

协程是轻量级的线程，它们能够在单一线程中并发运行，且不会像传统的线程那样消耗过多的资源。协程本质上是挂起函数，它允许在执行过程中暂停，并且能够恢复执行。

### **🔹协程的基本概念**

- **挂起函数（Suspending Function）**：是协程的核心，允许协程暂停并恢复执行。挂起函数通常以 `suspend` 关键字声明。
- **协程作用域（Coroutine Scope）**：协程在一个特定的作用域内运行，确保协程的生命周期与作用域相关联。协程作用域的任务可以并行执行，并且在作用域结束时，所有的协程都会被取消。
- **协程调度器（Dispatcher）**：负责控制协程在哪个线程上执行。例如，`Dispatchers.Main` 表示协程在主线程执行，`Dispatchers.IO` 用于 I/O 操作。
- **挂起点（Suspension Point）**：当协程执行到某个挂起函数时，它会在该点暂停，挂起函数可能会等待某些异步操作完成后恢复执行。

### **🔹 使用协程的基本步骤**

#### **(1)添加协程依赖**

首先，你需要在 `build.gradle` 文件中添加协程库依赖：

```xml
// 协程基础库
implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-core:1.5.0'

// Android 特定的协程支持库
implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-android:1.5.0'
```

#### **(2)启动协程**

你可以在不同的协程作用域内启动协程。`GlobalScope` 是最简单的方式，但在 Android 中不推荐使用。推荐在 `ViewModel` 中使用 `viewModelScope`，或者在 `Activity` 和 `Fragment` 中使用 `lifecycleScope`。

**使用 `GlobalScope` 启动一个协程（不推荐在 UI 线程中使用）：**

```kotlin
fun main() {
    // 在后台线程启动协程
    GlobalScope.launch {
        println("Start Coroutine")
        delay(1000L) // 延迟 1 秒
        println("End Coroutine")
    }
}
```

- **`launch`**：启动一个协程，返回一个 `Job` 对象，用于管理协程的生命周期。
- **`delay`**：挂起当前协程，但不会阻塞线程，其他协程可以继续运行。

#### **(3)挂起函数**

协程中的挂起函数是协程的核心，可以暂停协程的执行，并在异步操作完成后恢复。

```kotlin
fun main() = runBlocking {  // runBlocking 用于在主线程启动协程
    println("Start Coroutine")
    val result = fetchDataFromNetwork()
    println("Data fetched: $result")
    println("End Coroutine")
}

suspend fun fetchDataFromNetwork(): String {
    delay(2000L) // 模拟网络请求延迟
    return "Network Data"
}
```

- **`runBlocking`**：用于启动一个协程并阻塞当前线程，通常在顶层函数中使用来启动协程。
- **`suspend`**：修饰挂起函数，将该该函数挂起使其可以在**runBlocking**协程作用域中调用。
- **`delay`**：不会阻塞线程，它只会挂起当前协程。

#### **(4)使用不同的协程作用域**

在 Android 开发中，我们通常会使用 `lifecycleScope` 和 `viewModelScope` 来启动协程。

**在 `Activity` 或 `Fragment` 中使用 `lifecycleScope`：**

```kotlin
lifecycleScope.launch {
    val result = fetchDataFromNetwork()
    // 更新 UI
    textView.text = result
}
```

- **`lifecycleScope`**：与 Activity 或 Fragment 的生命周期绑定，协程会在生命周期结束时自动取消。

**在 `ViewModel` 中使用 `viewModelScope`：**

```kotlin
    fun fetchData() {
        viewModelScope.launch {
            val result = fetchDataFromNetwork()
            // 更新 UI 或 LiveData
            _data.value = result
        }
    }
}
```

- **`viewModelScope`**：与 ViewModel 的生命周期绑定，当 ViewModel 被清除时，所有协程会自动取消。

#### **(5)使用 `async` 和 `await` 进行并行操作**

`async` 和 `await` 用于同时启动多个协程，并在完成时返回结果。`async` 会返回一个 `Deferred` 对象，`await` 用来获取返回值。

```kotlin
fun main() = runBlocking {
    val deferred1 = async { fetchDataFromNetwork() }
    val deferred2 = async { fetchDataFromNetwork() }

    // 等待两个协程都完成
    val result1 = deferred1.await()
    val result2 = deferred2.await()

    println("Result 1: $result1")
    println("Result 2: $result2")
}

suspend fun fetchDataFromNetwork(): String {
    delay(1000L)
    return "Data from network"
}
```

- **`async`**：启动一个并发协程并返回 `Deferred` 对象。
- **`await`**：等待协程的结果。

#### **(6)使用 `withContext` 切换协程上下文**

有时你需要切换协程执行的线程上下文，例如在网络请求后将结果更新到 UI 线程上。

```kotlin
fun main() = runBlocking {
    val result = withContext(Dispatchers.IO) {
        fetchDataFromNetwork()  // 在 IO 线程执行
    }
    withContext(Dispatchers.Main) {
        println("Data fetched: $result")  // 更新 UI
    }
}

suspend fun fetchDataFromNetwork(): String {
    delay(2000L)  // 模拟网络请求延迟
    return "Network Data"
}
```

- **`withContext(Dispatchers.IO)`**：切换到 I/O 线程来执行耗时操作。
- **`withContext(Dispatchers.Main)`**：切换到主线程（UI 线程）来更新 UI。

### **🔹协程异常处理**

Kotlin 协程还提供了异常处理机制。你可以使用 `try-catch` 块来捕获协程内部的异常。

```kotlin
fun main() = runBlocking {
    try {
        launch {
            // 这里会抛出异常
            throw Exception("Something went wrong")
        }
    } catch (e: Exception) {
        println("Caught exception: ${e.message}")
    }
}
```

### **🔹协程的取消**

协程支持取消操作。通过调用 `Job.cancel()` 方法可以取消协程，`cancel()` 会使协程挂起并且不会继续执行。

```kotlin
val job = GlobalScope.launch {
    repeat(1000) { i ->
        println("Coroutine $i")
        delay(500L)
    }
}

job.cancel()  // 取消协程
```

---



## 26.DSL构建专有的语法结构

DSL 是专为特定领域（如数据库查询、配置文件、UI布局等）设计的编程语言或语言的一个子集。Kotlin 提供了丰富的支持，帮助开发者用 Kotlin 代码构建出自己的 DSL 语言结构，以使得代码更加易读和易写。

- **扩展函数**：你可以通过扩展函数给现有类添加自定义功能，从而实现新的语法结构。
- **高阶函数**：Kotlin 允许将函数作为参数传递给其他函数，这使得定义和构建 DSL 成为可能。
- **Lambda 表达式**：你可以通过 lambda 表达式使代码更加简洁，形成类似声明式的语法。
- **智能类型推断**：这使得你无需显式声明类型，语法更清晰，推导更自然。



### 🔹**Lambda with Receiver（接收者 lambda）**

接收者 lambda 允许在 lambda 中访问外部对象的成员，而不需要显式的引用对象。通过这种方式，你可以通过一个语法块来访问和操作对象，类似于嵌套调用的结构。

```kotlin
class DSL {
    fun sayHello() = println("Hello, World!")
}

fun dslExample(init: DSL.() -> Unit) {
    val dsl = DSL()
    dsl.init()
}

fun main() {
    dslExample {
        sayHello()  // 直接调用 DSL 中的方法，类似于在 "this" 上调用方法
    }
}
```

在这个例子中，`init` 是一个扩展函数 `DSL.() -> Unit`，其中 `DSL` 是接收者类型，而 `init` 是一个 lambda 表达式，它可以直接访问 `DSL` 类的成员。

### 🔹**构建层次结构**

你可以使用 lambda 表达式来构建一个嵌套的结构，这在构建树形结构或配置文件时特别有用。例如，构建 UI 或 HTML 模板时，嵌套的 DSL 会让代码看起来更简洁。

```kotlin
class HtmlBuilder {
    private val children = mutableListOf<String>()

    fun div(init: HtmlBuilder.() -> Unit) {
        val builder = HtmlBuilder()
        builder.init()
        children.add("<div>${builder}</div>")
    }

    override fun toString(): String = children.joinToString("")
}

fun html(init: HtmlBuilder.() -> Unit): String {
    val builder = HtmlBuilder()
    builder.init()
    return builder.toString()
}

fun main() {
    val htmlContent = html {
        div {
            div {
                println("Nested Div")
            }
        }
    }

    println(htmlContent)  // 输出 <div><div><div>Nested Div</div></div></div>
}
```

这里，我们创建了一个简单的 HTML 构建器 DSL。`div` 函数会在 `HtmlBuilder` 类中创建一个新的 `div` 标签，并可以嵌套其他标签。你可以在外部 `html` 块中定义结构，从而形成一个层次化的结构。

### 🔹**操作符重载**

在 Kotlin 中，你可以重载操作符，创建自定义的语法。这让你能够使用自然的语法来表达一些操作。

```kotlin
class Vector(val x: Int, val y: Int) {
    operator fun plus(v: Vector) = Vector(x + v.x, y + v.y)
}

fun main() {
    val vector1 = Vector(1, 2)
    val vector2 = Vector(3, 4)
    val result = vector1 + vector2  // 使用重载的 plus 操作符
    println("Result: (${result.x}, ${result.y})")  // 输出 Result: (4, 6)
}
```

在这个例子中，通过重载 `plus` 操作符，你可以使用 `+` 来直接操作自定义的类 `Vector`，使得代码更具有直观性和简洁性。

### 🔹 Kotlin 中构建 DSL 的常见应用

- **HTML DSL**：用于生成 HTML 文档，类似于上述的例子，可以通过嵌套的 lambda 构建 HTML 标签。
- **UI 构建器**：许多 UI 框架（如 Jetpack Compose）使用 DSL 来简化 UI 组件的声明式布局。
- **配置文件**：可以为 JSON、YAML 或 XML 等配置格式创建 DSL，使其更加自然易读。
- **构建工具**：如 Gradle 就是使用 Kotlin DSL 来定义构建脚本。

### 🔹示例：Kotlin DSL 构建数据库查询

假设你想要构建一个用于数据库查询的 DSL，使得查询语句更加优雅和简洁。

```kotlin
class QueryBuilder {
    private val conditions = mutableListOf<String>()

    fun where(condition: String) {
        conditions.add("WHERE $condition")
    }

    fun and(condition: String) {
        conditions.add("AND $condition")
    }

    fun or(condition: String) {
        conditions.add("OR $condition")
    }

    fun build(): String = "SELECT * FROM table ${conditions.joinToString(" ")}"
}

fun query(init: QueryBuilder.() -> Unit): String {
    val builder = QueryBuilder()
    builder.init()
    return builder.build()
}

fun main() {
    val query = query {
        where("age > 30")
        and("name = 'John'")
    }

    println(query)  // 输出: SELECT * FROM table WHERE age > 30 AND name = 'John'
}
```

---



