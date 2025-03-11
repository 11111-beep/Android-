# Java学习笔记总结



### 1.JDK编译java过程(用记事本)

![image-20241205185413173](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20241205185413173.png)

---



### 2.关键字

![image-20241206162133188](C:/Users/ASUS/AppData/Roaming/Typora/typora-user-images/image-20241206162133188.png)

---



### 3.数据类型

![image-20241206193540193](C:/Users/ASUS/AppData/Roaming/Typora/typora-user-images/image-20241206193540193.png)

---



### 4.标识符命名

![image-20241206194601334](C:/Users/ASUS/AppData/Roaming/Typora/typora-user-images/image-20241206194601334.png)

---



### 5.Scanner键盘录入(string+int)

![image-20241206195319479](C:/Users/ASUS/AppData/Roaming/Typora/typora-user-images/image-20241206195319479.png)

---



### 6.简化输入

#### psvm

#### sout

#### souf

#### 5.fori

#### 6.数组名.fori

---



### 7.方法

```java
public static void 方法名(){
    方法体(就是打包起来的代码);
}
```

---



### 8.面向对象

![image-20241208225156950](C:/Users/ASUS/AppData/Roaming/Typora/typora-user-images/image-20241208225156950.png)

---



### 9.private关键字

![image-20241209153416249](C:/Users/ASUS/AppData/Roaming/Typora/typora-user-images/image-20241209153416249.png)

![image-20241209153508233](C:/Users/ASUS/AppData/Roaming/Typora/typora-user-images/image-20241209153508233.png)

---



### 10.标准JavaBean类

![image-20241209195234145](C:/Users/ASUS/AppData/Roaming/Typora/typora-user-images/image-20241209195234145.png)

---



### 11.面向对象基本结构

#### 1.测试类(运行)

#### 2.JavaBean类(用插件)

#### 3.方法类(构建static方法(静态方法))

#### static静态方法不能调用非静态,但非静态方法可以调用所有

![image-20241210202833635](C:/Users/ASUS/AppData/Roaming/Typora/typora-user-images/image-20241210202833635.png)

![image-20241210203036395](C:/Users/ASUS/AppData/Roaming/Typora/typora-user-images/image-20241210203036395.png)

---





### 12.封装,继承与多态

#### 1. **封装（Encapsulation）**

**定义**：

- 将数据和操作封装在对象内部，通过对外提供有限的访问方式（如方法），隐藏实现细节。
- 使用**访问控制修饰符**（`private`、`protected`、`public`）保护对象的内部状态。

**好处**：

- 提高安全性，防止外部直接修改对象内部数据。
- 便于代码维护。

**示例**：

```
java复制代码class BankAccount {
    private double balance; // 私有属性，外部无法直接访问

    public BankAccount(double initialBalance) {
        this.balance = initialBalance;
    }

    // 提供受控访问的公共方法
    public double getBalance() {
        return balance;
    }

    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        } else {
            System.out.println("Invalid deposit amount");
        }
    }

    public void withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
        } else {
            System.out.println("Invalid withdrawal amount");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        BankAccount account = new BankAccount(1000);
        account.deposit(500);
        account.withdraw(200);
        System.out.println("Balance: " + account.getBalance()); // Balance: 1300
    }
}
```

------



#### 2. **继承（Inheritance）**

**定义**：

- 通过继承，子类可以**复用**父类的属性和方法，避免重复代码。
- 子类可以**扩展或重写**父类的方法。

**好处**：

- 提高代码的复用性。
- 子类可以根据需要添加新的功能，增强扩展性。

**示例**：

```
java复制代码// 父类
class Animal {
    protected String name;

    public Animal(String name) {
        this.name = name;
    }

    public void eat() {
        System.out.println(name + " is eating.");
    }
}

// 子类
class Dog extends Animal {
    public Dog(String name) {
        super(name); // 调用父类构造方法
    }

    // 子类特有方法
    public void bark() {
        System.out.println(name + " is barking.");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog("Buddy");
        dog.eat();  // 调用继承自父类的方法
        dog.bark(); // 调用子类自己的方法
    }
}
```

![image-20241210213011420](C:/Users/ASUS/AppData/Roaming/Typora/typora-user-images/image-20241210213011420.png)

---



### 继承中的重名调用

![image-20241211153930009](C:/Users/ASUS/AppData/Roaming/Typora/typora-user-images/image-20241211153930009.png)

---

### 继承中的方法重写

![image-20241211190419562](C:/Users/ASUS/AppData/Roaming/Typora/typora-user-images/image-20241211190419562.png)

---





#### 3. **多态（Polymorphism）**

**定义**：

- 多态指的是同一个方法在不同对象中有不同的表现形式。
- **编译时多态（静态绑定）**：方法重载（`overloading`）。
- **运行时多态（动态绑定）**：方法重写（`overriding`）和接口实现。

**好处**：

- 提高代码灵活性，通过同一个接口操作不同类型的对象。
- 增强扩展性，便于新增功能。

**示例**：

```
// 父类
class Animal {
    public void speak() {
        System.out.println("Animal makes a sound.");
    }
}

// 子类
class Dog extends Animal {
    @Override
    public void speak() {
        System.out.println("Dog barks.");
    }
}

class Cat extends Animal {
    @Override
    public void speak() {
        System.out.println("Cat meows.");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal myAnimal; // 使用父类引用指向子类对象

        myAnimal = new Dog();
        myAnimal.speak(); // Dog barks.

        myAnimal = new Cat();
        myAnimal.speak(); // Cat meows.
    }
}
```

------

![image-20241211203907489](C:/Users/ASUS/AppData/Roaming/Typora/typora-user-images/image-20241211203907489.png)

---



![image-20241211213315828](C:/Users/ASUS/AppData/Roaming/Typora/typora-user-images/image-20241211213315828.png)

![image-20241211220418215](C:/Users/ASUS/AppData/Roaming/Typora/typora-user-images/image-20241211220418215.png)

### 区别与联系

| 特性     | 定义                                   | 作用                 | 示例                                               |
| -------- | -------------------------------------- | -------------------- | -------------------------------------------------- |
| **封装** | 将数据和方法封装在类中，隐藏实现细节   | 提高安全性和可维护性 | 通过`private`保护属性，只能通过`getter/setter`访问 |
| **继承** | 子类复用父类的代码，支持代码重用和扩展 | 提高代码复用性       | 子类继承父类的属性和方法，支持扩展或重写           |
| **多态** | 同一操作在不同对象中表现出不同形式     | 提高灵活性           | 子类重写父类方法，父类引用调用子类实现             |

------



### 总结

- **封装**：保护和隐藏数据，实现安全性和简化接口。
- **继承**：实现代码复用和扩展，增强程序可维护性。
- **多态**：通过统一接口操作不同类型的对象，实现灵活性和扩展性。

---





### 13.new创建对象

在Java中，需要`new`一个新的对象的情况通常包括以下几类：

#### 1. **创建类的实例**

- 当你需要一个类的对象时，使用`new`创建实例。

```
class Person {
    String name;
    int age;
}

Person person = new Person(); // 创建一个新的Person对象
person.name = "Alice";
person.age = 20;
```

------

#### 2. **使用数组**

- 声明一个数组需要使用`new`来分配内存。

```
int[] numbers = new int[5]; // 创建一个长度为5的int数组
String[] names = new String[]{"Alice", "Bob"}; // 使用数组字面量创建
```

------

#### 3. **创建集合类（或其他库类）的实例**

- Java的集合类（如`ArrayList`、`HashMap`）通常需要使用`new`来初始化。

```
import java.util.ArrayList;
ArrayList<String> list = new ArrayList<>(); // 创建一个ArrayList对象
list.add("Hello");
```

------

#### 4. **自定义构造新对象**

- 通过`new`调用类的构造函数，可以初始化带参数或带默认值的对象。

```
class Circle {
    double radius;

    Circle(double r) { 
        this.radius = r; 
    }
}

Circle circle = new Circle(5.0); // 创建一个半径为5的Circle对象
```

------

### 5. **动态创建对象**

- #### 需要在运行时生成对象时，可以使用`new`关键字。

```
Object obj = new Object(); // 创建一个通用对象
```

------

#### 6. **内存分配或深拷贝**

- 如果需要独立于原对象的副本，可以使用`new`创建。

```
String original = "Hello";
String copy = new String(original); // 创建一个独立的字符串对象
```

------

#### 7. **匿名类或Lambda表达式**

- `new`可以用来创建匿名类实例。

```
Runnable task = new Runnable() {
    @Override
    public void run() {
        System.out.println("Running...");
    }
};
new Thread(task).start();
```

------

#### 特殊情况

- **不需要`new`的时候：**

  - **基本类型**：如`int`、`float`，直接使用值。
  - **字符串**：可以直接使用字面量（`"Hello"`），不需要使用`new`。

  ```
  String greeting = "Hello"; // 字符串字面量
  ```
  
- **工厂方法**：某些类有静态工厂方法（如`Integer.valueOf`）来创建对象，无需使用`new`。

  ```
  Integer num = Integer.valueOf(42); // 静态方法创建对象
  ```

---

### 14.包

#### 包名:公司域名的反写+包的作用(英文小写)

#### 导包:点击对象,ALT+回车

![image-20241212090543981](C:/Users/ASUS/AppData/Roaming/Typora/typora-user-images/image-20241212090543981.png)

---



### 15.final

![image-20241212141442604](C:/Users/ASUS/AppData/Roaming/Typora/typora-user-images/image-20241212141442604.png)

---



### 16.权限修饰符

![image-20241212141542958](C:/Users/ASUS/AppData/Roaming/Typora/typora-user-images/image-20241212141542958.png)

---



### 17.static{ }静态代码块(只执行一次,并且在构造之前执行)

![image-20241212145331954](C:/Users/ASUS/AppData/Roaming/Typora/typora-user-images/image-20241212145331954.png)

![image-20241212145504833](C:/Users/ASUS/AppData/Roaming/Typora/typora-user-images/image-20241212145504833.png)

#### 一般用于数据的初始化

---



### 18.接口(interface)

![image-20241212201606572](C:/Users/ASUS/AppData/Roaming/Typora/typora-user-images/image-20241212201606572.png)

#### ALT+回车

---





### 18.适配器原理

![image-20241212225039034](C:/Users/ASUS/AppData/Roaming/Typora/typora-user-images/image-20241212225039034.png)

新建一个java类implements 接口,在测试类调用时再继承这个类

---



### 19.内部类

![image-20241213205542094](C:/Users/ASUS/AppData/Roaming/Typora/typora-user-images/image-20241213205542094.png)

![image-20241213211909076](C:/Users/ASUS/AppData/Roaming/Typora/typora-user-images/image-20241213211909076.png)

---



### 20.LambdaDemo

![image-20241213224713174](C:/Users/ASUS/AppData/Roaming/Typora/typora-user-images/image-20241213224713174.png)



![image-20241213221750123](C:/Users/ASUS/AppData/Roaming/Typora/typora-user-images/image-20241213221750123.png)

.![image-20241213222047043](C:/Users/ASUS/AppData/Roaming/Typora/typora-user-images/image-20241213222047043.png)

![image-20241213223235651](C:/Users/ASUS/AppData/Roaming/Typora/typora-user-images/image-20241213223235651.png)

---



