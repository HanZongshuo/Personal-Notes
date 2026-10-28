# 1. 输出语句
```
换行输出： println(输出内容);
不换行输出 : print(输出内容);
假如多行代码写到一行,中间需要加分号,后面可以不加分号;
```
# 2. 注释
```scala
// 这样是单行注释
/**/ 多行注释
```
# 3. 变量
```
定义格式：var /val 变量名：变量类型 = 初始值
val name: String = "Scala"   //不可变变量val
var age: Int = 25  //可变变量
```
由于`scala`属于一种静态语言，支持类型推导，因此可以省略掉变量类型让编译器自行推断。

在JVM 中，每一个`Boolean(java.lang.Boolean)`、`Int(java.lang.Integer)`、`String` 都代表一个类，因此比如想要获取一个布尔类型的类别，则需要先调用JVM中的getclass方法，然后在调用getSimpleName()，因为这个方法是对于类生效。

```
val judge = true
println(s"The type of judge: ${judge.getClass.getSimpleName}")
```

类型转换：`toString`,`toInt`,`toDouble`.
# 4. 字符串
```
val name = "Alice"
val age = 25
println(s"My name is $name and I am $age years old.")  
// 输出：My name is Alice and I am 25 years old.
```

在`scala`中，每一个表达式都会返回一个值，即使是代码块也是一个表达式
```
val result = {
val a = 10
val b = 20
a+b
}
println(result)

val fullname = {  
  val firstname = "Han"  
  val lastname = "Zongshuo"  
  firstname + " " + lastname  
}  
println(fullname)
```
# IF 表达语句

IF表达式都是可以直接返回值的，比如
```
val category =  if(age>=18)"Adult" else "Minor"
println(category)  //Adult
```

IF链式表达式
```
val score = 85
val grade = if(score >=90)"A"
			else if (score>=80) "B"
			else if(score>=70) "C"
			else "F"   //注意必须有else才算完整，否则会报错
println(grade) //B
```



# 5.  函数与方法的定义
```
def functionName(x:Int):Int = {  //这里的int是返回类型的
// 方法
}
```
这样可以定义匿名函数：
```
val mult =(x:int)=>{x*x}
```
函数是一个实体，而不仅仅是写在对象内的代码，可以作为参数传递。函数可以赋值，而方法不可以
```
class Calculator {
  // 方法：是类定义的一部分
  def max(x: Int, y: Int): Int = {
    if (x > y) x else y
  }
  // 函数：是一个只读属性，持有函数引用
  val increment = (x: Int) => x + 1
}

val c = new Calculator()
// increment 本身就是一个对象
val alias = c.increment  // ✓ 完全没问题

// 可以传递
List(1, 2, 3).map(alias)  // List(2, 3, 4)

// 可以存储
val funcs = List(alias, alias)
funcs.foreach(f => println(f(10)))  // 11, 11
```



scala中class 和object的区别：
![[Pasted image 20250311130104.png]]
## 5.2 对象和类

```
class Calculator {
	def max(x: Int, y: Int): Int = {
	    if (x > y) x else y
	}
	val increment = (x: Int) => x + 1
}
object CalculatorApp extends App {
	val c = new Calculator()  //对于一个类需要先new一个
	val a = c.max(2, 3)
	println(a)
	val b = c.increment(3)
	println(b)
}

//对于对象就不需要new
object MathUtils {
	 def add(x: Int, y: Int): Int = x + y 
	}
MathUtils.add(2, 3)	
```

方法提升

把方法提升为函数，这样就可以进行正常的传递。
```
val c = new Calculator()
val func:(Int,Int) =>Int =  c.max  //这样func就是把方法变成了函数
val func = c.max_ //或者这样进行提升
```

## Closure
Closure：类似于global的概念，指的是使用一个或者多个自由变量的函数，该函数的返回值依赖这些自由变量[Scala | Closures - GeeksforGeeks](https://www.geeksforgeeks.org/scala/scala-closures/)

```scala
var factor = 3 //自由变量
val multiplier = (x: Int) => x * factor

println(multiplier(5))  // 输出 15

factor = 4   //改变参数之后，调用函数的运行结果也会发生变化
println(multiplier(5))  // 输出 20（factor 已改变）

```

示例代码2， 替换整个闭包函数：
```
object ClosureExamplesApp2 {

    var increment = (x: Int) => x + 1  // 初始的 `increment` 函数，简单加 1

    def main(args: Array[String]): Unit = {
        println(increment(5))  // 输出 6
        resetFunc()            // 调用 `resetFunc()`，改变 `increment` 的定义
        println(increment(5))  // 输出 55
    }

    def resetFunc() {
        // 本地变量 `addition` 被捕获并存储在堆上
        var addition = 50
        increment = (a: Int) => {   // 新的 `increment` 闭包
            a + addition
        }
    }
}

```
这里调用resetFunc()之后，相当于将increment函数改变了，因此输出结果也变了

# 6. Higher Order Function:
接受函数作为参数，并且返回的同样是函数。
[Scala Tutorial 13 - Scala - Higher Order Functions](https://www.youtube.com/watch?v=ohKDVmCMKEg)
```
def applyFunc(f: Int => Int, value: Int): Int = { //这里表示接受一个函数（把Int变成Int），接受一个Int类型数值
  f(value)  // f 是一个函数参数，调用f把函数value传进去
}

// 可以传入任何符合 Int => Int 类型的函数
applyFunc((x: Int) => x * 2, 5)    // 10
applyFunc((x: Int) => x * x, 5)    // 25
applyFunc((y: Int) => y, 5)        // 5

也可以使用第二种方式函数定义出来
val apply = (n:Int,f:Int => Int) => f(n)

val f1 = (x:Int) =>x*x
println(apply(5,f1)) //25
```

## 6.1 filter 和 foreach，map
函数定义
```scala
def filter(p: (A) => Boolean): List[A]  //对其中符合A函数的添加至List中
def foreach(f: (A) => Unit): Unit  //对每一个元素执行操作，不返回

val nums = List(1,2,3,4)
val even = nums.filter(x=>x%2 == 0)  //List(2,4)
val all = nums.filter(x=>true) // List(1,2,3,4) 这个永远成立
nums.foreach(x=>println(x)) //打印每一个输出
```

如果想要有相应的返回，则可以使用map。如果对List 使用.map 则会返回一个List 出来
```
val numbers = List(1, 2, 3, 4, 5) 
val doubled = numbers.map(_ * 2) 
println(numbers) // List(1, 2, 3, 4, 5) - 原列表不变! 
println(doubled) // List(2, 4, 6, 8, 10) - 新列表
```

还有一个例子
```scala
def formatResult(n:String,x:Int,f:Int=>Int):String = {
    val msg = "The %s of %d is %d "
    msg.format(n,x,f(x))  //这里是格式化的意思
  }
  def factorial(x:Int):Int = {
    if(x == 1) 1
    else x* factorial(x-1)
  }
  val String = formatResult("factorial",3,factorial)
  println(String)
```

## 6.2 部分应用函数
```scala
def operation(x:Int,y:Int):Int = {x*y}  
val doubleIt = operation(2,_:Int)  //我相当于占住一个参数的位置了,这个位置的类型是Int，剩下只需要输入一个值即可  
println(doubleIt(3))
```
## 6.3 Curring
现在不需要占位符了，直接分开传入，更加灵活
```
def operation(x:Int)(y:Int):Int = {x*y}  
println("First Method")  
println(operation(2)(3))  
println("Second method")  
val step1 = operation(2)  
println(step1(3))
```

## 6.4 List定义
List， vector，array 等都属于sequence
1. 不可变List： 不可变 `List` 位于 `scala.collection.immutable` 包中，默认导入，无需额外引入。两个不可变的list可以连接
```scala
val list1 = List(1, 2, 3)  
val list2 = List(4, 5, 6) // 不可变列表  
  
//拼接列表  
val combinedList = list1 ++ list2 //或者使用:::  
println(combinedList)  
// 输出: List(1, 2, 3, 4, 5, 6)  
  
//也可以进行追加操作，但是在头部追加  
val newList = 0 :: list2 //输出: List(0, 4, 5, 6)  
println(newList)  
//在尾部添加  
println(newList :+ 3)
```
2. 一些运算符
```scala
//表示在空列表上添加元素
val list1 = List(1, 2, 3,4,5)  
println(s"Original list1: $list1")  
println(s"Length of list1: ${list1.length}")  
println(s"Reversed of list1: ${list1.reverse}")  
println(s"Drop first element: ${list1.drop(1)}")  
println(s"The first element:${list1.head}, last element:${list1.last}")  
println(s"The tailed list:${list1.tail}")  
println(s"The init part of list:${list1.init}")  
println(s"The  list is empty :${list1.isEmpty}")

Original list1: List(1, 2, 3, 4, 5)
Length of list1: 5
Reversed of list1: List(5, 4, 3, 2, 1)
Drop first element: List(2, 3, 4, 5)
The first element:1, last element:5
The tailed list:List(2, 3, 4, 5)
The init part of list:List(1, 2, 3, 4)
The  list is empty :false
```

- `::` → 从右向左读取（右结合），只适合list
    
- `+:` → 在**头部**添加元素, 也适用于vector
    
- `:+` → 在**尾部**追加元素（O(n)）

3. `.mkString` 方法将 `List` 的内容转换为字符串，并使用指定的分隔符将每个元素连接起来。`List` 是一种可迭代集合（Iterable Collection），因此可以对其中的元素进行处理。
```
val list1 = List(1, 2, 3,4,5)  
val strList1 = list1.mkString(";") //按照设定的进行划分，转化为String类型  
println(s"Converted:${strList1}")
```

## 6.5 `foldLeft` 和 `foldRight`
- 对于**较大的列表**，推荐使用 `foldLeft`，因为它更高效。可以配合reverse一起使用
```
// foldLeft：从左到右
val list1 = List(1, 2, 3,4,5)  
val strList1 = list1.mkString(";") //按照设定的进行划分，转化为String类型  
val sumRes = list1.foldLeft(0)(_+_)  
val sumString = list1.foldLeft("")(_+_)  
println(sumRes)  //把列表中的加起来  
println(sumString)  //把列表中加到""里
```

## 6.6 `flatten` 和`flatMap` 
```scala
//flatten的
  val nestList = List(
    List(1,2,3),
    List(4,5)
  )
  println(nestList.flatten)
  //flatmap 就相当于map + flatten
  val nums = List(1,2,3)
  val step1 = nums.map(x=>List(x,10*x))  //List(List(1,10),List(2,20),List(3,30))
  val step2 = step1.flatten
  println(step2)
  //一步成功的
  val step3 = nums.flatMap(x=>List(x,10*x)) 
  println(step3)  //List(1, 10, 2, 20, 3, 30)
```

## 6.7 自定义类型
```
case class Person(var name:String, var age:Int)  //自定义
  
val dad = new Person("John",45)  
val mom = new Person("Anny",36)  
  
val family = List(dad,mom)  
family.foreach("Family member:" +println(_))  //直接使用_进行指代  
val over30 =  family.filter(_.age>30)  
println(over30)  
  
val ages = family.map(_.age)  
val averageAge = ages.sum /ages.size  
println("Average age:" + averageAge)
```

## 6.8 哈希表
注意顺序并不是固定的
```
import scala.collection.immutable.HashMap  
object exercise4 extends App{  
  val capitalCity = HashMap(  
    "UK" ->"London",  
    "FRANCE" ->"Paris",  
    "Spain" ->"Madrid"  
  )  
  println(capitalCity.keys) //Set(Spain, UK, FRANCE)  
  println(capitalCity.values) //Iterable(Madrid, London, Paris)  
  println(capitalCity.getOrElse("China","Not Know"))  
}
```

## 6.9 Vector
类似于树状结构，访问和存储，插入都很快
```
val vector = Vector(1, 2, 3)
vector :+ 4
```

## 6.10 Queue
自定义queue
```scala
class Queue[T](val head:T, val tail:List[T]){  
  def enqueue(x:T) = new Queue(x,head::tail)  //当加入一个新的元素的时候，新加入的x作为头元素，然后原先的head接在tail之前，作为尾元素  
  def peek = head  
  def dequeue = new Queue(tail.head,tail.tail)  //出队  
  override def toString =  
    s"$head: ${(tail mkString ",")}"  
}  
object exercise5 extends App{  
  val q = new Queue[String]("John",List("Dany","Phoebe"))  
  println("q = "+q) //q = John: Dany,Phoebe  
  println("q's peek:" + q.peek) // q's peek:John  
  val q2 = q.dequeue    
  println("q2: "+q2)   //q2: Dany: Phoebe  
  println("q = " + q)  //q = John: Dany,Phoebe  
}
```

# 7. Variance
scala的三种规则，协变`[+T]` 可以使用子类替换父类, 逆变`[-T]`，可以用父类替换子类，不变`[T]` 。
也可以加入一些限制，

比如下界，代表加入进来的必须是比T更通用的类型
```
def enqueue[U >: T](x: U) = new Queue(x, _head :: tail)
```

以及上界，加入的需要是T的子类型
```
def enqueue[U <: T](x: U) = new Queue(x, _head :: tail)
```

## 7.2 模式匹配
```scala
val scores = 90  
val grade = scores match{  //第一种用法  
  case 1  => "first"  
  case 2 =>"second"  
  case 3 => "third"  
  case _ => scores +"th"  
}  
println(grade)  
case class Person(name:String,age:Int)  
val bob = Person("Bob",33)  
val personGreeting = bob match{  
  case Person(n,a) => "Hi " + n +",It is your " + a + " birthday"  
  case _ =>"Something else"  
}  
println(personGreeting)
```
另一个例子，列表求和
```
sealed trait List[+A] //定义一个接口List  
case object Nil extends List[Nothing]  
case class Cons[+A](head:A,tail: List[A]) extends List[A]  
  
object exercise5 extends App{  
  def sum(nums:List[Int]):Int = nums match{  
    case Nil => 0  
    case Cons(x,xs) =>x + sum(xs)  
  }  
}
```

# 8. 抽象类和抽象字段
## 8.1 抽象类
- 抽象字段： 没有初始化值
- 抽象方法： 没有方法体
```Scala
abstract class Shape{  
  def area(): Double //抽象方法： 不知道怎么计算，因此这里不写方法的具体内容。  
}  
class Square(var edge: Double) extends Shape{  
  override def area(): Double = edge * edge  
}  
  
class Rectangle(var length: Double, var width: Double) extends Shape{  
  override def area(): Double = length * width  
}  
  
def main(args: Array[String]): Unit = {  
  val s1 =  new Square(5)  
  println(s"Square: ${s1.area()}")  
  
  val s2 = new Rectangle(3,2)  
  println(s"Rectangular: ${s2.area()}")  
}
```

## 8.2 抽象字段：


[07.抽象字段_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Q5411t74z?spm_id_from=333.788.videopod.episodes&vd_source=dfd5ab52d1797f02d8cd2499a5d71f61&p=78)
	`上述视频中有如何进行快速改写父类的抽象字段，Alt + 回车`
	

```Scala
abstract class Person{  
  val occupation : String  
}  
// 在这里需要重写父类的所有内容，因为父类是一个抽象类， 快捷键为光标放在Student，Alt + 回车  
class Student extends Person{  
  override val occupation: String = "Student"  
}  
  
class Teacher extends Person {  
  override val occupation: String = "Teacher"  
}  
  
def main(args: Array[String]): Unit = {  
  val t = new Teacher  
  println(t.occupation)  
  
  val s = new Student  
  println(s.occupation)  
  
}
```
## 7.3 匿名对象类

- 当对对象方法仅调用一次的时候
- 可以作为方法的参数进行传递
- 本质上是一个子类对象，因此需要对抽象父类内部的内容全部重写

```Scala
abstract class Person{  
  def sayHello(): Unit  
}  
  
def show(p: Person): Unit = {  
  p.sayHello()  
}  
  
def main(args: Array[String]): Unit = {  
  // 场景1： 假如在这个时候我想要调用一下Person的方法，创建一个新的对象重写方法太麻烦了，因此直接匿名内部类  
  //new Person(){  
  //      override def sayHello(): Unit = println("Hello, scala.")  //    } 这里相当于是创建了匿名对象类，即创建了 一个子类对象，实际还需要.方法来实际调用具体的方法  
  new Person(){  
    override def sayHello(): Unit = println("Hello, scenario 1.")  
  }.sayHello()  
  
  //使用场景2：作为方法的实际参数进行传递  
  //现在我想要调用这个show方法，但是实际上需要有一个person类的对象  
  //因此我通过使用匿名内部类的方法来创建Person的抽象子类对象  
  val p:Person = new Person(){ //多态  
    override def sayHello(): Unit = println("Hello, scenario 2.")  
  }  
  show(p)  
}
```


## 7.4 特质
可以单独写出某些对象的单独功能。类与类之间只能单继承，但是类与特质之间既可以单继承也可以多继承。
- 继承单个特质
```scala
trait Logger{  
  def log(msg:String): Unit  
}  
  
class ConsoleLogger extends Logger{  
  override def log(msg: String): Unit = println(msg)  
}  
  
def main(args: Array[String]): Unit = {  
  val c1 = new ConsoleLogger  
  c1.log("Class extends trait.")  
}
```
- 类继承多个特质

```scala
trait MessageSender{  
  def send(msg:String) :Unit  
}  
trait MessageReceiver{  
  def receive():Unit  
}  
  //注意这里的关键字
class MessageWorker extends MessageReceiver with MessageSender{  
  override def send(msg: String): Unit = println(s"Send the message ${msg}")  
  
  override def receive(): Unit = println("Receive successfully.")  
  
}  
  
def main(args: Array[String]): Unit = {  
  val m = new MessageWorker  
  m.send("Hello")  
  m.receive()  
  
}
```
- 单例对象继承特质
```scala
trait Logger{  
  def log(msg:String):Unit  
}  
trait  Warning{  
  def warn(msg:String):Unit  
}  
object ConsoleLogger extends Logger with Warning{  
  override def log(msg: String): Unit = println(s"Log information: ${msg}")  
  override def warn(msg: String): Unit = println(s"Warning information: ${msg}")  
}  
  
def main(args: Array[String]): Unit = {  
  ConsoleLogger.log("I am just a log information")  
  ConsoleLogger.log("I am just warning information")  
}
```

## 7.5 隐式转换

使用implicit 关键字声明带有单个参数的方法，自动被调用以此来实现将某种数据类型转化为另一种数据类型
步骤：
1. 在object对象中， 定义隐式转换的方法
2. 需要隐式转化的地方导入隐式转化， 类似于导入包

# 8. 泛型类和参数化

主构造器：
```Scala
object work5 {  
  class Person(val name: String = "Han", val age: Int = 23){ //默认情况  
    println("The constructor had been used.")  
  }  
  
  def main(args: Array[String]): Unit = {  
    val p1 = new Person() //如果不写的话就是使用默认的情况，  
    println(s"The name is ${p1.name} and age is ${p1.age}")  
  
    val p2 = new Person(age = 20)  
    println(s"The name is ${p2.name} and age is ${p2.age}")  
  }  
}
```

如果想要访问类中的成员，必须先要new一个新成员，但是如果是在一个单例对象内，只需要`单例对象名.`即可访问. 上面和下面是两种主程序入口，一个是创建`main方法`， 另一个是通过创建`单例extends APP`

```scala
package com.jjh.scala.collection

class Queue[T](val head: T, val tail: List[T]) {

  // 入队（enqueue）：创建一个新队列，新元素作为新的 head，原队列变为 tail
  def enqueue(x: T): Queue[T] = new Queue(x, head :: tail)

  // 查看队列头部元素（peek）
  def peek: T = head

  // 出队（dequeue）：移除当前 head，返回新的 Queue
  def dequeue: Queue[T] = 
    if (tail.isEmpty) throw new NoSuchElementException("Queue is empty")
    else new Queue(tail.head, tail.tail)

  // 重写 toString 方法，方便打印
  override def toString: String = 
    s"head: $head, tail: [${tail.mkString(",")}]"
}

object QueueTest extends App {
  val q = new Queue[String]("John", List("Denise", "Phoebe"))

  println("q = " + q)    // 打印原始队列 --这里可以隐式调用toString,因为q是Queue[T]类型
  println("q.peek = " + q.peek)  // 查看队列头部元素（peek）

  val q2 = q.dequeue  // 进行出队操作，返回新队列
  println("q2 = " + q2) // 打印出队后的新队列
  println("q = " + q)   // 确保原队列未被修改
}

```


**泛型方法**：T 代表类型，一般使用T代表任意类型
```
def getMiddleElement[T](arr:Array[T]) = arr(arr.length/2)

//程序入口
println(getMiddleElement(Array(1,2,3,4,5)))
```

**泛型类：**可以使得内部的字段并不固定

```
class Pair[T](val a:T, val b:T)
val p3 = new Pair[Int](10,20) //在这里进行指定类型即可
```

比如创建一个队列
```
class Queue[T](val head: T, val tail: List[T]) {
  def enqueue(x: T): Queue[T] = new Queue(x, head :: tail) //用于插入值到队列之中

  def peek: T = head

  def dequeue: Queue[T] = new Queue(tail.head, tail.tail)

  override def toString: String = s"head: $head, tail: ${tail.mkString(",")}"
}

val q = new Queue(1, List(2, 3, 4))  // 创建队列 Queue(1, List(2,3,4))
println(q.peek)                    //输出1
val q2 = q.enqueue(0)                // 插入 0
println(q2)                           // 输出 head: 0, tail: 1,2,3,4


```


我们可以**定义泛型类型**，使其具有：

- **协变（Covariant）**：从**更具体**的类型到**更通用**的类型（`Queue[+T]`）
```Scala
class Queue[+T](val elements: List[T]) {
  def getElements: List[T] = elements //返回一个elments类型是List
}

val stringQueue: Queue[String] = new Queue(List("hello", "world"))
val anyRefQueue: Queue[AnyRef] = stringQueue  // ✅ 可以赋值
//意味着我们把一个String类型的变成了范围更大的Anyref类型
```
- **逆变（Contravariant）**：从**更通用**的类型到**更具体**的类型（`Queue[-T]`）
```scala
class Printer[-T] {
  def print(value: T): Unit = println(value)
}

val anyRefPrinter: Printer[AnyRef] = new Printer[AnyRef]
val stringPrinter: Printer[String] = anyRefPrinter  // ✅ 可以赋值

```

在Scala中，使用**类型边界（Type Bounds）**可以限制泛型类型参数 `T` 的范围，以确保只使用特定的类型或其父类型。这是通过**上界（Upper Bound）**和**下界（Lower Bound）**来实现的。

---

### **🔹 上界（Upper Bound）**

- **上界**限制泛型参数 `T` 必须是某个类型的子类型。
    
- 如果我们想让泛型参数 `T` 只接受某个类型或它的子类型（即 **`T` 的上界**），可以使用上界来指定。例如，`T <: Person` 表示 `T` 必须是 `Person` 或 `Person` 的子类型。
    

### **🔹 下界（Lower Bound）**

- **下界**限制泛型参数 `T` 必须是某个类型的父类型。
    
- 如果我们想让泛型参数 `T` 只接受某个类型或它的父类型（即 **`T` 的下界**），可以使用下界来指定。例如，`T >: Person` 表示 `T` 必须是 `Person` 或 `Person` 的父类型。

# 9. 错误处理

### 🌱 为什么 Scala 避免抛出异常？

- **抛异常是副作用（Side Effect）**：函数就不再纯粹，容易影响程序的可预测性和测试性。
    
- **破坏引用透明性**：如果一个函数有时返回结果，有时抛出异常，就不是“相同输入 → 相同输出”了。
    
- **影响组合性**：无法像正常值一样传递或组合异常。

比较推荐的做法是使用option，也就是告诉系统函数可能会失败
```
def divide(x:Int,y:Int):Option[Int] = {  
  if(y == 0) None  
  else Some(x/y)  
}  
println(divide(10,2))  
println(divide(10,0))
```
基础的option搭配函数的有`map,flatMap,getOrElse,orElse,filter`

改进的办法：
```
def safeDivide(a: Int, b: Int): Option[Int] = {
  try {
    Some(a / b)
  } catch {
    case e: ArithmeticException => None // 出错了不崩溃，而是返回 None
  }
}
```

# 10

## 10.1 尾递归

**尾递归**指的是：

> **一个函数的最后一步是调用自己，并且调用完后，不需要再做任何其他事情。**

好处就是：**可以让编译器帮你把递归改成循环，避免内存爆栈**！

因为普通递归，每一次调用，都会把函数信息压到**调用栈**（Call Stack）里。  
如果递归太深，比如处理一个超大列表（几百万个元素），**普通递归会爆栈 StackOverflowError**！

但**尾递归**，因为**最后一步是直接递归自己**，编译器可以直接**复用当前栈帧**，就像在一个循环里做的事情，内存不会增长，所以非常安全。

`@tailrec` 是一个Scala的**编译器指令**（annotation），告诉编译器：

> “嘿，检查一下这个函数是不是标准的尾递归。”

```
def factorial(n:Int):Int = {  
  @annotation.tailrec  
    def go(n:Int,acc:Int):Int = {  
    if (n<=0) acc  
    else go(n-1,acc * n)  
  }  
  go(n,1) //提供一个调用接口  
}  
println(factorial(5))
```

## 考前复习
```
def addition(x:Int,y:Int):Int = {  
  if (y == 0) x  
  else if(y<0) addition(x-1,y+1)  
  else addition(x+1,y-1)  
}  
println(addition(4,-2))  
  
def subtration(x:Int,y:Int):Int = {  
  addition(x,-y)  
}  
println(subtration(4,-2))  
  
def factorial(x:Int):Either[String,Int] = {  
  @tailrec  
  def loop(n:Int,acc:Int):Either[String,Int] = {  
    if(n<0) Left("Error")  
    else if (n==0) Right(acc)  
    else loop(n-1,n*acc)  
  }  
  loop(x,1)  
}  
println(factorial(5))  
println(factorial(-3))  
  
def isPrime(nums:List[Int]):List[Boolean] = {  
  def isPrimeHelper(n:Int,divisor:Int):Boolean = {  
    if(n<2) false  
    else if(divisor * divisor > n) true  
    else if(n%divisor == 0) false  
    else isPrimeHelper(n,divisor+1)  
  }  
  nums.map(x=>isPrimeHelper(x,2))  
  
}  
println(isPrime(List(1,2,3,4,5,8,9)))
```