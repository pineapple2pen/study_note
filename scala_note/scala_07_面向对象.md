### 面向对象

#### 类与对象

##### 养猫问题

​	张奶奶样了一群猫，一只小白白色3岁，小花花色10岁。请编写一个程序，用户输入小猫名字就显示猫的名字年龄花色，如果名字输出错误则显示无该猫。
```scala
package com.dongbo.faceobj

import scala.io.StdIn
import util.control.Breaks._

object CatDemo {
  def main(args: Array[String]): Unit = {
    val cat1 = new Cat
    cat1.age = 3
    cat1.name = "小白"
    cat1.color = "白"
    val cat2 = new Cat
    cat2.color = "花"
    cat2.name = "小花"
    cat2.age = 10
    breakable {
      do {
        println("请输入猫的名字。（x退出）")
        val name: String = StdIn.readLine()
        if (name == "x") {
          break()
        }
        if (name == cat1.name) cat1.printToString()
        else if (name == cat2.name) cat2.printToString()
        else println("无该猫")

      } while (true)
    }
  }
}

class Cat {
  //声明属性
  // 当声明了一个var name:String 时，在底层对应为  private name
  // 同时会生成两个public 的name()  get和set方法
  var name: String = ""
  var age: Int = _ //_ 表示默认值
  var color: String = _

  def printToString(): Unit = {
    printf("cat name %s cat age %d cat color %s\n", this.name, this.age, this.color)
  }

}


```
##### 说明

1. Java是面向对象的编程语言，由于历史原因，Java中还存在非面向对象的内容：基本类型、null、静态方法等；
2. scala语言源自Java，所以其天生就是面向对象的语言，而且Scala是纯粹的面向对象语言，即在Scala中一切皆为对象

##### 类与对象的区别

1. 类是抽象的，概念的，代表一类事物，比如人类，猫类...
2. 对象是具体的，实际的，代表一个具体事物
3. 类是对象的模板，对象是类的一个个体，对应一个实例
4. Scala中类与对象的区别和联系和Java是一样的

##### 基本语法

```
[修饰符]class Xxx{
	...
}
```

##### 注意事项

1. Scala中，类并不声明为public，所有的这些类都具有共有可见性（默认为 public）
2. 一个Scala中可以包含多个类

##### 属性

属性是类的一个组成部分，一般是值类型数据，也可以是引用类型，比如之前猫的name、age、color就是属性

###### 属性注意事项

1. 属性的定义语法与变量相同。`[访问修饰符] var 属性名称[: 类型] = 默认属性值`
2. 属性的定义类型可以为任意类型，包含值类型或引用类型
3. Scala中声明一个属性，必须显示的初始化，然后根据初始化数据的类型自动推断，属性类型可以省略
4. 如果赋值为null，则一定要加类型，如果不加类型，则会自动推断为该属性的类型为Null型。
5. 如果在定义属性的时候，暂时不想赋值，也可以使用下划线`_`，让系统默认分配默认值。
类型|_对应的值
:--:|:--:
Byte Short Int Long|0
Float Double|0.0
String 和引用类型|null
Boolean|false

6. 不同对象的属性是独立的，互不影响，一个对象对属性的更改不影响另一个。

##### 对象创建

###### 语法

```
val | var 对象名[:类型] = new 类型()
```

###### 说明

1. 如果不希望改变对象的引用（内存地址），应该声明为val性质，否则声明为var。Scala设计者推荐使用val，因为一般来说，在程序中，只改变对象属性的值，而不是改变对象的引用。
2. Scala在声明对象变量时，可根据创建对象的类型自动推导，所以类型声明可省略。但是当类型和new 对象类型有继承关系（多态）,就必须写类型了。

##### 内存分配机制

与java相同

```scala
package com.dongbo.faceobj

object CatDemo {
  def main(args: Array[String]): Unit = {
    val cat = new Cat
    val cat2 = cat
    cat.name = "小白"
    println("name " + cat2.name)// name 小白
  }
}

class Cat {
  var name: String = ""
  }

}


```

---

#### 方法

Scala中的方法其实就是函数。（方法的声明方式于函数相同）

```scala
package com.dongbo.faceobj

object MethodDemo {
  def main(args: Array[String]): Unit = {
    val cat = new Cat
    cat.name = "Honey"
    cat.miao
    cat.eat("fish")
  }

}

class Cat {
  var name: String = ""

  def miao = "miao miao miao ~~~~"

  def eat(food: String): Unit = {
    printf(this.name + " eat " + food)
  }

}
```

##### 方法调用机制

1. 当我们Scala开始执行的时候，会现在栈区开辟一个main栈。main栈是最后被销毁的
2. 当Scala程序在执行到一个方法的时候，总会开辟一个新栈
3. 每个栈是独立的空间，变量（基本数据类型）是独立的，互相不影响（引用类型除外）
4. 当方法执行完毕之后，该方法开辟的栈就会被JVM回收

---

#### 构造器

在Java中，创建对象时，往往需要传参进行对象的初始化，Scala也不例外。

##### java构造器的特点

1. 在Java中一个类可以包含多个构造方法，构造方法重载
2. 如果没有定义构造方法，系统会默认给类生成一个默认无参构造方法，也叫 默认构造器。
3. 一旦定义了自己的构造方法，默认的构造方法也就被覆盖了，就不能再默认使用无参的构造器，除非显示的定义一下

##### Scala构造器介绍

Scala类的构造器包含主构造器和辅助构造器

###### 语法

```
class 类名（形参列表）{ // 主构造器
  // 辅助构造器函数的名称为this，可以有多个，编译器通过不同参数来区分
  def this(形参列表){ // 辅助构造器
  
  }
  def this(形参列表2){// 可以有多个辅助构造器
  
  }
}
```

###### 代码演示

```scala
class Person(inName: String, inAge: Int){
  var name:String = inName
  var age: Int = inAge
  var sex:Char = _
}
```

###### 注意细节

1. Scala构造器作用是完成对新对象的初始化，构造器没有返回值

2. 主构造器的声明直接置放于类名之后

3. 主构造器会执行类定义中的所有语句，这里可以体会到Scala的函数编程和面向对象编程融合再一起，即构造器也是方法，参数传递和使用方法和前面的函数部分没有区别

4. 如果主构造器无参数，小括号可以省略，构建对象时调用的构造方法的小括号也可以省略

5. 辅助构造器的名称为this，多个辅助构造器通过不同参数列表进行区分
```scala
class Person(inName: String, inAge: Int){
  var name:String = inName
  var age: Int = inAge
  var sex:Char = _

  def this(inName: String, inAge: Int, inSex:Char){
    // 辅助构造器必须在第一行显示的调用主构造器
    this(inName, inAge)
    this.name = inName
    this.age = inAge
    this.sex = inSex
  }

}
```

6. 如果想让主构造器变成私有的，可以在（）之前加上private，这样用户就只能通过辅助构造器来构造对象了
7. 辅助构造器的声明不能和主构造器的声明一致，否则会报错

---

##### 属性（进阶）

1. Scala类的主构造函数的形参未使用任何修饰符修饰，那么这个参数是局部变量
2. 如果参数使用val关键字声明，那么Scala会将参数作为类的私有只读属性使用
3. 如果参数使用var关键字声明，那么Scala会将参数作为类的成员属性使用，并会在底层提供属性对应的get\set方法，即这个时候成员属性是私有的，但是可以读写
```scala
object ConDemo {

  def main(args: Array[String]): Unit = {
    val c1 = new Class01("Tom")
    println(c1.name)

    val c2 = new Class02("Jake")
    println(c2.name)
    // 可读写
    c2.name = "Jake Zhou"
    
    val c3 = new Class03("Bob")
    // 只能读
    println(c3.inName)
  }
}

class Class01(inName: String){
  var name:String = inName
}
class Class02(var name: String){
}
class Class03(val inName: String){
}
```

###### Bean属性

JavaBeans规范定义了Java的属性是像getXxx()和setXxx()的方法。许多Java工具（框架）都依赖这个命名习惯。为了Java的互操作性，将Scala字段加@BeanPropety时，这样就会生成规范的get/set方法。这个时候可以使用get/set方法来对属性进行操作

```scala
import scala.beans.BeanProperty
class Class04(inName: String){
  @BeanProperty var name: String = inName
}
```

##### 对象创建的流程

1. 加载类的信息（属性方法，方法信息）
2. 在内存（堆）开辟空间
3. 使用主构造器对属性进行初始化（若有父类则会在此前会先用父类的主构造器进行初始化）
4. 使用辅助构造器对属性进行初始化
5. 最后将开辟的内存地址赋值给引用名称
```scala

object Gzq {
  def main(args: Array[String]): Unit = {
    val o = new Obj2("Tom")
    println(o.name)
  }
}

class Obj1 {
  var name: String = _
  println("obj1 z")

  def this(inName: String) {
    this
    println("obj1 fz")
    this.name = inName
  }
}

class Obj2 private() extends Obj1 {
  println("obj2 z")

  def this(inName: String) {
    this
    println("obj2 fz")
    this.name = inName
  }
}
/*
obj1 z
obj2 z
obj2 fz
Tom
*/
```

---

### 包（package）

#### Java包

##### 作用

1. 区分相同名字的类
2. 当类很多时，可以很好的管理类
3. 控制访问范围 

语法

```
package com.dongbo;
```

实际就是创建不同的文件夹来保存文件。

#### Scala包

#### 基本介绍

和Java一样，Scala中项目管理可以使用包，但是Scala中的包功能更加强大，使用也更加复杂。

Scala包的语法和作用于Java一样，但是Scala中包名和源码所在系统所在目录结构可以不一致，但是编译后的字节码文件和包名所在的目录会一致（这个事情会由编译器完成）

#### Scala会自动引入的包

java.lang.*

scala包（但是scala下面的子包还是需要自己手动导入使用）

Predef包（预定义包）

#### 注意事项和细节

1. Scala进行package打包时，可以有如下形式：

```scala
// 常规写法
package com.dongbo.test

class Dongbo{
    
}
```

```scala
// 分割写法和常规写法的最终效果完全一样
package com.dongbo
package test

class Dongbo{
    
}
```

```scala
// 大括号
// 说明：
// package com.dongbo{} 表示创建了包com.dongbo,在{}中可以继续创建它的子包，还可以写类、特质trait,还可以写object
// 即Scala支持在一个文件中，可以同时创建多个包，以及给各个包创建类、trait和object
package com.dongbo{// 包com.dongbo
    package test{// 包 com.dongbo.test
        class Dongbo{// 包com.dongbo.test下创建类Dongbo
    
	    }
        
        object DongboTest { // 包com.dongbo.test下创建object
            
        }
    }
}
```

2. 包也可以向嵌套类那样嵌套使用（打包方式3），这样使用的好处是：程序员可以在同一个文件中将类、特质等创建在不同的包中。使用非常灵活
3. 作作用域原则： 可以直接向上访问。即Scala中子包中直接访问父包中的内容，大括号体现作用域。
4. 父包要访问子包的内容时，需要import对应的类。
5. 可以在同一个Scala文件中声明并并列多个package（推荐嵌套的package不要超过三层）
6. 包名可以相对也可以绝对。比如在访问BeanPropety的绝对路径是：`_root_.scala.beans.BeanPropety`。在一般情况下，我们使用相对路径来引入包，只有当包名相冲突的时候，使用绝对路径来处理。
```scala
import scala.beans.BeanPropety
class Manange(var name: String) {
    // 使用相对路径引入包之后使用
    @BeanPropety var age = _
    // 直接使用相对路径引用
    @scala.beans.BeanPropety var sex:Char = _
    // 使用绝对路径引用，可以避免包名冲突
    // _root_为从根路径开始查找
    @_root_.scala.beans.BeanPropety var address: String = _
    
}
```

#### 包对象

包可以包含类、对象和特质trait,但是不能包含函数、方法和变量的定义，这个是Java虚拟机的限制。为了这样做Scala提出了包对象的概念来解决这个问题。

```scala
// 1.在包中直接写方法，或者定义变量就错误，但是可以用包对象的方法来解决
// 2.package object scala表示创建一个包对象scala,他是com.dongbo这个包对应的包对象
// 3.每一个包都可以有一个包对象
// 4.包对象的名字需要和子包一样
// 5.在包对象中可以定义变量，方法
// 6.在包对象中定义的变量和方法，就可以在对应的包中使用
// 7.在底层，这个包对象会生成两个类（package$.class 和 package.class）
package com.dongbo {
  package object scala{
    
  }
}
```

#### 包的可见性

Java中提供了4种访问控制修饰符，用来控制控制方法和变量的访问权限：

1. 公开级别：public,对外公开
2. 受保护级别： protected,对子类和同一个包中的类公开
3. 默认级别： 没有修饰符，相同一个包的类公开
4. 私有级别： private，只有类本身可以访问，不对外公开

看一段Scala的示例

```scala
package com.dongbo.scalaStudy.faceobj {

  class PakgDemo {
    var name: String = "JJ"
    private var isHuman = true
    
    def info(): Unit ={
      printf("%s ", name)
    }
  }

  object PakgDemo {
    def main(args: Array[String]): Unit = {
      test(new PakgDemo)
    }
    def test(p:PakgDemo): Unit ={
      printf(p.isHuman.toString)
    }
  }

}
```

当一个文件中出现了class 和object的同一个类对象名则：

1. class Xxx是伴生类
2. object Xxx是伴生对象
3. 因为Scala设计者将static拿掉了，于是有了伴生类于半生对象的概念
4. 伴生类一般写非静态的内容，伴生对象就是静态的内容

##### Scala中包可见性和访问修饰符的使用

1. 当属性的访问权限为默认的时候，从底层来看属性是private的，但是因为提供了 `xxx_$eq()//类似setter`和`xxx()//类似getter`方法，因此从使用效果来看是任何地方都可以访问
2. 当方法访问权限为默认的时候，默认为pubilc访问权限
3. private为私有权限，只在类的内部和半生对象中可以使用
4. protected为受保护权限，Scala中受保护权限比Java更为严格，只能自雷吧访问，同包无法访问
5. 在Scala中没有public关键字，既不能使用public显示的修饰属性和方法
6. 包访问权限（表示属性有了限制，同时包也有了限制），这点可Java不一样，体现出scala的灵活性

```scala
class P{
    private[dongbo] val name = _ //增加包访问权限之后，private同时起作用，不仅同类可以使用，同时增加了访问标识的包下的其他类也可以访问
}
```

#### 包的引入

Scala引入包也是使用import，基本的原理和机制和Java一样，但是Scala中的import功能更加强大也更加灵活

由于Scala语言源自于Java，所以java.lang包中的类会自动的引入到当前的环境中，而Scala中的scala包和Predef包的类也会自动引入到当前的环境当中，即这几个包中的类都可以任意使用。

##### 细节和注意

1. 在Scala中，import语句可以出现在任何地方，并不仅限于文件顶部，import语句的作用一直延伸到包含该语句的块末尾。这种语法的好处是：在需要时，再去引用包，缩小import范围，提高效率
2. Java中，如果想要导入包中的所有类，可以通过通配符*，Scala中采用下划线`_`
3. 如果不想要某个包中的全部类，而是指定几个类，可以使用选取器（大括号{}）

```scala
import scala.collection.mutable.{HashMap, HashSet}
```

4. 如果引入多个包中含有相同的类，那么可以将不需要的类进行重命名进行区分

```scala
import java.util.{HashMap=>JavaHashMap, List}
import scala.collection.mutable.{HashMap, HashSet}
```

5. 如果某个冲突的类不会使用到，那么这个类可以隐藏掉

```scala
//第一个_表示将HashMap影藏，第二个表示再引入所有
import java.util.{HashMap=>_, _}
import scala.collection.mutable.{HashMap, HashSet}
```

---

### 面向对象的三大特征

封装、继承、多态

#### 封装

封装就是把抽象出来的数据和对数据的操作封装在一起，数据被保护在内部，程序的其他部分只有通过被授权的操作（成员方法）才能对数据进行操作

1. 封装可以隐藏细节
2. 在对数据进行操作的时候，可以对数据进行验证
3. 同时可以加入业务逻辑

##### 如何封装

1. 将属性进行私有化
2. 提供一个共有的set方法，用于对属性判断并赋值

```scala
def setXxx(参数名: 类型):Unit={
    //参数值验证
    //赋值
}
```

3. 提供一个共有的get方法，用于获取属性的值

```
def getXxx():返回类型={
  return xxx
}
```

##### 封装的注意事项和细节

1. Scala中为了简化代码的开发，当声明属性的时候，本身就自动提供了对应的setter/getter方法，如果属性声明为private的，那么自动生成的setter/getter方法也是private的，如果属性省略的权限修饰符，那么自动生成的setter/getter方法是public的
2. 如果支队一个属性进行简单的set和get，只要声明一下该属性（属性使用默认访问修饰符）不用写专门的getset方法，默认会创建，访问的时候，直接对象.变量，这样也是为了保持访问的一致性
3. 从形式上来看，dog.food直接访问属性，其实底层任然是访问的方法。

#### 继承

##### java继承的回顾

```
class 子类 extends 父类{
//todo
}
```

继承可以解决代码的复用，可以在编程的时候更加符合人类的思维。当多个类存在相同的属性（变量）和方法的时候，可以从这些类中抽象出父类，在父类中定义这些相同的属性和方法，所有的子类不用再重新定义这些方法和属性吧，只需要继承就可以了。

##### scala的继承

语法上于Java没有差别

案例：

```scala
package com.dongbo.scalaStudy.faceobj

object ExtendDemo {

  def main(args: Array[String]): Unit = {
    val b = new ChainStudent('男', "Bob")
    printf("name is %s, sex is %s", b.name, b.sex)
  }

}

class EarthHuman{
  var sex: Char = _
  var name:String = _

}

class ChainStudent private() extends EarthHuman{
  def this(inSex: Char, inName:String){
    this
    this.sex = inSex
    this.name = inName
  }
}
```

##### 重写方法

Scala明确规定，重写一个非抽象的方法需要使用@Override修饰符，调用超类的方法使用super()关键字

```scala
class EarthHuman{
  var sex: Char = _
  var name:String = _

  def printName(): Unit ={
    println("name=" + name)
  }

}

class ChainStudent private() extends EarthHuman{
  def this(inSex: Char, inName:String){
    this
    this.sex = inSex
    this.name = inName
  }

  override def printName(): Unit = {
    print("student ")
    super.printName()
  }
}
```

##### Scala中类型的检查和转换

要测试某个对象是否属于某个给定的类，可以使用isInstancesOf方法。使用asIntanceOf方法将引用转换为子类的引用。classOf获取对象的类名。

1. `classOf[String]` 就如同Java的`String.class`
2. `obj.isInstancesOf[T]`就如同Java的`obj isInstancesOf T `判断obj是不是T类型
3. `obj.asIntanceOf[T]`就如同于Java的`(T) obj`，将obj类型强制转换为T类型。

##### Scala中超类的构造

1. 类有一个主构造器和任意个辅助构造器，而每个辅助构造器都必须先调用主构造器（也可以时间接调用）
2. 只有主构造器可以调用父类的构造器。辅助构造器不能直接调用父类的构造器。再Scala的构造器中，你不能调用`super（param）`

```scala
class EarthHuman{
  var sex: Char = _
  var name:String = _
  def this(inName:String){
    this
    name = inName
  }
}
class ChainStudent(inName:String) extends EarthHuman(inName){
  def this(inSex: Char, inName:String){
    this
    this.sex = inSex
    this.name = inName
  }
}
```

##### 覆写字段

在Scala中，子类改写父类的字段，称为**覆写字段**，覆写字段需要使用`override`修饰符。Java是不支持覆写字段的，在Java里面如果父类和子类中定义了同一个字段，不会报错，只是在实例化子类时，父类的字段被隐藏了。但是可使用父类的引用调用父类的该字段

###### 细节

1. def只能重写另一个def（方法只能重写另一个方法）
2. val只能重写另一个val属性或重写不带参数的def，或重写一个不带参数的def
3. var只能重写另一个抽象的var属性

抽象属性：声明没有初始化的变量就是抽象的属性，抽象属性在抽象类：

1. 一个属性没有初始化，那么这个属性就是抽象属性
2. 抽象属性在编译成字节码文件的时候，属性并不会声明，但是会自动生成抽象方法，所以类必须声明为抽象类
3. 如果覆写一个父类的抽象属性，那么override关键字就可以省略（父类的抽象属性生成的是抽象方法，所以就不涉及到重写的概念）

##### 抽象类

在Scala中，通过abstract关键字标记不能被实例化的类。方法不用标记abstract，只要省略掉方法体即可。抽象类可以拥有抽象字段，抽象字段就是没有初始值的字段

```scala
abstract class An {
  //抽象属性
  var name: String
  //普通属性
  var color: String = _
  // 抽象方法
  def t()
}
```

###### 作用

抽象类的价值更多在于设计，是设计值设计好了之后，让子类继承并实现抽象类

###### 细节

1. 抽象类不能被实例化
2. 抽象类不一定要包含abstract方法。也就是说，抽象类可以没有abstract方法
3. 一旦类包含了抽象方法或者抽象属性，那么这个类就必须是抽象类
4. 抽象方法不能有主体，不必使用abstract修饰
5. 如果一个类继承了抽象类，则它必须实现抽象类的所有抽象方法和抽象属性，除非它自己也是抽象类
6. 抽象方法和抽象属性不能使用private、final来修饰，因为这些关键字都是重写/实现相违背的
7. 抽象类中可以有实现的方法
8. 子类重写抽象方法不需要override，写上也不会报错

##### 匿名子类

通过包含带有定义或者重写的代码块的方式创建一个匿名的子类

###### Java

```java
public class NonName {

    public static void main(String[] args) {

        NonClass nonName = new NonClass() {
            @Override
            public void test() {
                System.out.println("test");
            }
        };
        nonName.test();
    }
}

abstract class NonClass {
    abstract public void test();
}
```

###### Scala匿名子类

```scala
package com.dongbo.scalastudy

object NonClassScala {
  def main(args: Array[String]): Unit = {
    val noName: NonNameClass = new NonNameClass {

      override val test: String = "55"

      override def cry(): Unit = {
        println("test")
      }
    }

    noName.cry()
  }
}

abstract class NonNameClass() {
  val test: String

  def cry()
}
```

##### 继承层级

1. 在scala中，所有类都是AnyRef的子类，类似Java的object
2. AnyRef和AnyVal都扩展于Any类，Any类是根节点
3. Any中定义了isInstanceOf、asInstanceOf方法以及哈希方法
4. Null类型的唯一实例是null对象。可以将null值赋值给任意引用，但不能赋值给值类型的变量
5. Nothing类型没有实例，它对于泛型结构是有用的（Nil空列表）

#### 多态

略

---

### 伴生对象

​    当单例对象与某个类共享同一个名称时，它就被称为是这个类的伴生对象(companion object)。 
这个类可以访问伴生对象的所有成员。

​    在Java中，有静态对象这一概念，但是在Scala中，去掉了这一块的内容，但是为了兼容使用静态对象这一效果，引入了伴生对象这一概念。

#### 伴生对象小结

1. Scala中伴生对象采用object关键字声明，伴生对象中声明的全是静态的内容，可以使用对象名称直接调用
2. 伴生对象对应的类型称为伴生类，伴生对象的名称应该和伴生类一致
3. 伴生对象中的属性和方法都可以同归伴生对象（类名）直接调用
4. 从语法角度来看，所谓的伴生对象就是类的静态方法和成员的集合
5. 从技术角度来看，Scala还是没有生成静态的内容，只是将伴生对象生成了一个新的类实现属性和方法的调用
6. 从底层原理看，伴生对象实现静态内容是依赖于public static final MODULE$实现的
7. 伴生对象的声明应该和伴生类声明在同一个源码文件中（不在会报错）
8. 如果`class A`独立存在，那么A就是一个类，如果`object A`独立存在，那么A就是一个“静态”性质的对象

#### apply方法

当创建一个对象时，可以直接通过apply方法来实例化对象，不用使用new关键字。所以apply方法可以用来当做工厂函数，也可以快速的去实现一个单例对象。

###### 单例

使用apply实现一个懒汉式单例模式

```scala
object SignDemo {
  def main(args: Array[String]): Unit = {
    val d = God
    val d2 = God
    println(d == d2)
  }
}

class God private() {

}

object God {
  var god: God = null

  def apply(): God = {
    if (god == null) {
      god = new God()
    }
    god
  }
}
```

---

### 特质

Scala引入trait特征，第一可以替代Java的接口，第二也是对单继承的一种补充

##### 说明

1. Scala提供了trait特质，特质可以同时拥有抽象方法和具体方法，一个类可以实现/继承特质
2. 特质中没有实现的方法就是抽象方法。类通过extends继承特质，通过with继承多个特质
3. 所有的Java接口都可以当做特质使用

#### 动态混入

1. 除了可以在类声明时继承特质以外，还可以在构建对象时混入特质，扩展目标类的功能
2. 这种方式也可以应用于抽象类功能进行扩展
3. 动态混入是Scala特有的方式，可以在不修改类声明/定义的情况下。扩展类的功能，非常灵活，耦合性低
4. 动态混入可以在不影响原有继承关系的基础上，给指定的类扩展功能

```scala
package com.dongbo.scalastudy

object TraitMixDemo {
  def main(args: Array[String]): Unit = {
    // 在不修改类定义情况下，可以让他使用trait的方法
    val oracleDb = new OracleDb with Operator
    oracleDb.insert(5)

    // 抽象类有抽象方法如何动态混入
    val mysqlDb = new MysqlDb with Operator {
      override def connect(): Unit = {
        println("connect ...")
      }
    }
    mysqlDb.insert(6)
    mysqlDb.connect()
  }
}

trait Operator {
  def insert(id: Int): Unit = {
    println("insert " + id)
  }
}

class OracleDb {

}

abstract class MysqlDb {
  def connect()
}
```

##### 叠加特质

构建对象的时候混入多个特质，称为叠加特质。（特质声明顺序是从左到右，方法执行顺序是从右到左）

```scala
package com.dongbo.scalastudy


object TraitMixDemo {
  def main(args: Array[String]): Unit = {

    val e1 = new E with D
    e1.a()
    println("-------------------------------")
    val e2 = new E with C with D
    e2.a()
  }
}

trait A {
  def a()
}

trait B extends A {
  println("build B")
  override def a(): Unit = {
    println("B override A a() method")
  }
}

trait C extends B {
  println("build C")
  override def a(): Unit = {
    println("C override B a() method")
  }
}

trait D extends B {
  println("build D")
  override def a(): Unit = {
    println("D override B a() method, and run B a()")
    super.a()
  }
}

class E {

}

```

上面代码的运行结果

```shell
build B
build D
D override B a() method, and run B a()
B override A a() method
-------------------------------
build B
build C
build D
D override B a() method, and run B a()
C override B a() method
```

从代码的运行结果可以看出，当我们混入多个特质的时候，会以从左到右的顺序来混入特质。再执行方法时，会从右到左的方法优先级来执行。当执行到super的时候，是指的左边的特质，如果左边没有特质了，则才会去执行自身父特质的super方法。同时，这个对象也不再是实例化的对象，而是特质混入的一个新的对象类型。

###### 细节

1. 特质声明顺序为从左到右
2. Scala在执行叠加对象的方法时，会首先从后面的特质（从右到左）开始执行
3. Scala中特质中如果调用可super，并不是表示调用的父特质的方法，而是向前面（左边）继续查找特质，如果找不到，才会去父特质查找。
4. 如果想要调用具体特质的方法，可以指定`super[特质].xxx`其中的泛型必须是该特质的直接超类类型

```scala
//修改上面的代码之后，查看执行结果
trait D extends B {
  println("build D")
  override def a(): Unit = {
    println("D override B a() method, and run B a()")
    super[B].a()
  }
}
```

##### 在特质中重写抽象方法的一个特例

```scala
trait A {
  def a()
}

trait F extends A{
  /*
  * 1. 如果我们在子特质中重写/实现了一个父特质的抽象方法，但是同时调用了super
  * 2. 这个时候这个方法并不是完全实现，因此需要声明为 abstract override
  * 3. 但是在执行时，会很久动态混入的特性，不会调用父类的方法，而是左边的方法
  * 因此在使用时需要考虑动态混入的顺序。
  * */
  abstract override def a(): Unit = {
    println("F override A a()")
    super.a()
  }
}
```

可以这样理解，当我们给某个方法增加了abstract override后，就是明确的告诉编译器，该方法确实是重写了父特质的抽象方法，但是重写之后，该方法任然是一个抽象方法（因为没有完全的实现，需要其他特质来继续实现[通过混入顺序来控制]）

#### 富接口

当特质中既有抽象方法又有非抽象方法的时候，这个特质就叫富接口

#### 特质中的具体字段

特质中可以定义具体的字段，如果初始化了就是具体字段，如果不初始化就是抽象字段。混入该特质的类就具有了该字段，字段不是继承，而是直接加入类，成为了自己的字段。

#### 特质中的抽象字段

特质中未被初始化的字段在具体的子类中必须被重写

#### 特质构造的顺序

特质也是有构造器的，构造器中的内容由“字段的初始化”和一些其他语句构成。

第一种特质构造顺序（声明类的时候混入特质）

1. 调用当前类的超类构造器
2. 第一个特质的父特质构造器
3. 第一个特质构造器
4. 第二个特质构造器的父特质构造器，如果已经执行过就不再执行
5. 第二个特质构造器
6. 重复4）5）步骤（如有多个特质）
7. 当前类的构造器

例子：

```scala
package com.dongbo.scalaStudy.traitpkg
/*
* C
* A
* B
* D
* */
object TraitDemo {
  def main(args: Array[String]): Unit = {
    val d = new D
    println(d)
  }
}

trait A {
  println("A")
}

trait B extends A {
  println("B")
}

class C {
  println("C")
}

class D extends C with B {
  println("D")
}

```



第二种特质构造顺序（在构建对象时，动态混入特质）

1. 调用当前类的超类构造器
2. 当前类的构造器
3. 从左到右特质由父当当前按顺序执行

例子：略

所以第一种方式实际在构建类对象，在混入特质的时候，**该对象还没有创建**

第二种则是构造了一个匿名子类，可以理解为**在混入特质的时候，对象已经创建了**

#### 扩展类的特质

特质可有继承类，用来扩展类的一些功能

```scala
// TestException继承了Exception方法，那么它就可以使用Exception的方法了
trait TestException extends Exception{
  def log(): Unit ={
    println(getMessage())
  }
}
```

所有混入该特质的类，会自动成为那个特质所继承的超类

```scala
// 因为Test2Exception继承了TestException
// 而TestException继承了Exception
// 那么Test2Exception就是Exception的子类了
class Test2Exception extends TestException{
  override def getMessage: String = {
    "test..."
  }
}
```

需要注意的是，已经继承了另一个类（A）,则要求A是特质超类的子类，否则就造成了多继承现象，编译报错

```scala
//IndexOutOfBoundsException为Exception的子类
class Test3Exception extends IndexOutOfBoundsException with TestException{
  override def getMessage: String = {
    "getMessage"
  }
}
```

#### 自身类型

主要是为了解决特质的循环依赖问题，同时可以确保特质在不扩展某个类的情况下，依然可以做到限制混入该特质的类的类型

```scala
// 这个就是自身类型特质，当这里做了自身类型之后
// trait Test4Logger extends Exception,要求混入该特质的来也是Exception的子类
trait Test4Logger{
  // 明确告诉编译器，他就是Exception，如果没有这句话，下面的getMessage就无法使用
  this : Exception =>
  def log(): Unit = {
    println(getMessage)
  }
}
```

---

### 嵌套类

在Scala中，可以在任何的语法结构中嵌套任何语法结构。如在类中可以再定义一个类，这样的类就是嵌套类，其他语法结构也是一样的

嵌套类类似于Java中的内部类

#### Java的内部类回顾

##### 定义

在Java中，一个类的内部又完整的嵌套了另一个完整的类结构。被嵌套的类称为内部类（inner class），嵌套的其他类称为外部类。内部类最大的特点就是可以直接访问私有属性，并且可以体现类与类之间的包含关系

##### 语法

```java
class Outer { //外部类
    class Inner {
 		//内部类       
    }
}
class Other {
    // 外部其他类
}

```

##### 分类

- 从定义在外部类的成员位置上来看
  1. 成员内部类（没有使用static修饰）
  2. 静态内部类（使用static修饰）
- 定义在外部类局部位置上（方法内）来看：
  1. 分为局部内部类（有类名）
  2. 匿名内部类（匿名类，无类名）

##### 代码示例

```java
package com.dongbo.innerclasstest;

public class TestJavaClass {
    public static void main(String[] args) {
        // 创建一个外部类对象
        OuterClass outerClass1 = new OuterClass();
        // 创建一个外部类对象
        OuterClass outerClass2 = new OuterClass();
        // 创建成员内部类
        // Java中，将成员内部类当成一个属性，因此使用下面方式创建内部类
        OuterClass.InnerClass innerClass1 = outerClass1.new InnerClass();
        OuterClass.InnerClass innerClass2 = outerClass2.new InnerClass();
        // 下面的方法调用说明在Java中，内部类只和类型相关，也就是说只要是
        // OuterClass.InnerClass类型的对象就可以传给形参 InnerClass
        innerClass1.test(innerClass2);
        innerClass2.test(innerClass1);

        //创建静态内部类
        // 因为在Java中静态内部类是和类相关的，使用 new OuterClass.StaticInnerClass();
        OuterClass.StaticInnerClass staticInnerClass = new OuterClass.StaticInnerClass();

    }
}

class OuterClass{ //外部类
    class InnerClass{ // 成员内部类
        public void test(InnerClass ic){
            System.out.println("test: " + ic);
        }
    }

    static class StaticInnerClass{// 静态内部类

    }
}
```

####  Scala内部类

##### 语法

```scala
class OuterClass{ // 外部类
  var name:String = "Scala"
  class InnerClass{ // 内部类
    
  }
}

object OuterClass{ //伴生对象
  class StaticInnerClass{ // 静态内部类
    
  }
}
```

##### 代码示例

```scala
object InnerDemo {
  def main(args: Array[String]): Unit = {
    // 创建两个外部类的实例
    val outerClass1: OuterClass = new OuterClass
    val outerClass2: OuterClass = new OuterClass
    // 创建内部类
    // 创建内部类语法是 对象.内部类来创建的
    // 内部类实例和外部对象是有关联的
    val innerClass1 = new outerClass1.InnerClass
    val innerClass2 = new outerClass2.InnerClass
    
    // Scala创建静态内部类与Java 一样
    val staticInnerClass = new OuterClass.StaticInnerClass
  }
}
```

##### 使用

内部类如果想要访问外部类的属性，可以通过外部类对象访问(`外部类名.this.属性名`)

```scala
class OuterClass{
  var name:String = "Scala"
  class InnerClass{
    def info(): Unit ={
      println("Outer class name is " + OuterClass.this.name)
    }
  }
}
```

还有一种访问方式`外部类别名.属性名`

```scala
class OuterClass{
  myOuter => // 这样写可以理解成为 myOuter就是代表外部类别名的访问
  // 当给外部类指定别名的时候，需要将外部类的属性放在别名后面
  var name:String = "Scala"
  class InnerClass{
    def info(): Unit ={
      println(name)
      println(myOuter.name)

    }
  }
}
```

#### 类型投影

看下面这段代码

```scala
class OuterClassScala{
  myOuter=>
  class InnerClassScala{
    def test(ic: InnerClassScala): Unit ={
      System.out.println(ic)
    }
  }
}

object Test{
  def main(args: Array[String]): Unit = { 
    val staticInnerClass = new OuterClass.StaticInnerClass
    val o1 = new OuterClassScala
    val i1 = new o1.InnerClassScala
    // 正常
    i1.test(i1)
    val o2 = new OuterClassScala
    val i2 = new o2.InnerClassScala
    //正常
    i2.test(i2)
    // 报错
    // i2.test(i1)
  }
}
```

若希望上面报错地方正常，需要使用类型投影

```scala
class OuterClassScala{
  myOuter=>
  class InnerClassScala{
    // 使用 OuterClassScala#InnerClassScala 类型投影的作用就是屏蔽外部对象对内部对象的影响
    def test(ic: OuterClassScala#InnerClassScala): Unit ={
      System.out.println(ic)
    }
  }
}
```

##### 原因

​    Java中的内部类是属于外部类，因此在Java中方法调用自身对象就可以，因为是按照类型来匹配的。而Scala中内部类属于外部类的对象，所以外部类的对象不一样，创建出来的内部类也不一样，无法互换使用

  所以采用类型投影，Scala类型投影就是在方法声明上，如果使用 `外部类#内部类`的方式，表示忽略内部类的对象关系，等同于Java中内部类的操作语法。（即忽略对象的创建方式，只考虑类型）

---

### 练习

1. 定义一个Point类和一个伴生对象，可以不用new而直接用Point(3,4)来构造实例。

```scala
class Point(var x: Double, var y: Double) {
}

object Point {
  def apply( x1: Double = 0.0, y1: Double = 0.0): Point = new Point(x1, y1)
}
```

2. 编写一个Scala应用程序，使用APP特质，以反序打印命令参数，用空格隔开。

```scala
object NonClassScala extends App{
    println(args.reverse.mkString(" "))
// 不继承App的普通伴生对象使用参数方法
//  def main(args: Array[String]): Unit = {
//    println(args.reverse.mkString(" "))
//  }
}
```

3. 编写一个扑克牌4花色枚举，起toString方法分别返回四种花色。实现一个方法，判断是否为红色。

```scala
object Test{
  def main(args: Array[String]): Unit = {
    println(Suits)
    println(Suits.isRed(Suits.Diamond))
  }
}

object Suits extends Enumeration{
  // type 可以用于给类型起别名
  type Suits = Value
  val Spade = Value("♠")
  val Club = Value("♣")
  val Heart = Value("♥")
  val Diamond = Value("♦")

  override def toString(): String = {
    Suits.values.mkString(",")
  }
  def isRed(c:Suits) = c == Heart || c == Diamond
}
```

