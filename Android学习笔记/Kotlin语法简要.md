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

