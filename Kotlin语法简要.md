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

### if语句

if具有返回值,返回值就是if语句中最后一行代码的返回值

```kotlin
fun largerNumber(num1: Int, num2: Int) = if (num1>num2)num1 else num2
```

### when条件语句(类似于switch)

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

### 集合的创建与遍历

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

### 集合的函数API

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

### Java函数式API的使用

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

### 1. with

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



### 2. run

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



### 3. apply

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



### 总结

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



### 1.运用注解

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



### 2.运用顶层方法

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

### 1.接受函数作为参数

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

### 2.将函数作为返回值

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



### 1. noinline

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



### 2. crossinline

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
getSharedPreferences("data",Context.MODE_PRIVATE),edit {
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









