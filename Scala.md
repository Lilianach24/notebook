# Scala入门

## 1. 概述

1. Scala ---- Java++
   - Scala基于JVM, 和Java完全兼容,同样具有跨平台、可以执行号、方便的垃圾回收等特性
   - Scala比Java更加面向对象
   - Scala是一门函数式编程语言
2. Scala更适合大数据的处理
   - Scala对集合类型数据处理有费城好的支持
   - spark的底层用Scala编写
3. Scala语言特点
   - 是一门以Java虚拟机（JVM）为运行环境并将<font color="red">面向对象</font>和<font color="red">函数式编程</font>的最佳特性结合在一起的<font color="red">静态类型编程</font>语言（静态语言需要提前编译的如：Java、C、C++等，动态语言如：js）
   - Scala源代码(*.scala)会被编译成java字节码(*.class),然后运行于JVM上,<font color="red">并可以调用现有的java类库,实现两种语言的无缝衔接</font>
   - 简洁高效
   - 将函数式编程语言的特点融合到java中

## 2. Scala环境搭建

1. 安装步骤

   1. 首先安装jdk
   2. 下载对应的Scala安装文件
   3. 配置好环境变量

2. 编译scala源文件

   scalac *.scala

   scala *(文件名)

## 3. 编写代码

```scala
package chapter01

/**
 * object: 关键字，声明一个单例对象（伴生对象）
 */
object helloWorld {
  /**
   * main方法: 从外面可以直接调用执行的方法
   * def 方法名称(参数名称: 参数类型): 返回值类型 = {方法体}
   * @param args
   */
  def main(args: Array[String]): Unit = {
    println("hell world")
    System.out.println("hello world from scala")
  }
}
```

```scala
package chapter01

/**
 * 入口类
 * @param name
 * @param age
 */
class Student(name: String, age: Int) {
  def printInfo(): Unit = {
    println(name + " " + age + " " + Student.school)
  }
}

//引入伴生对象(伴生类)
object Student{
  val school: String = "atguigu"
  def main (args: Array[String] ): Unit = {

    val alice = new Student("alice", 20)
    val bob = new Student("bob", 23)
    alice.printInfo()
    bob.printInfo()
}
}
```



# 变量和数据类型

## 1. 注释

Scala注释使用和Java完全一样

## 2. 常量和变量

1. 基本语法

   var 变量名[: 变量类型]= 初始值		var i:Int = 10

   val 常量名[: 变量类型]= 初始值		val j:Int = 20

   <font color="red">注:能使用常量的地方不用变量</font>

   - 声明变量时,类型可以省略,编译器自动推导,即类型推导
   - 类型确定后,就不能修改,说明Scala时强数据类型语言
   - 变量声明时,必须要有初始值
   - 在声明一个变量时,可以使用var或val类修饰,var修饰的变量可以该边,val修饰的变量不可以改

## 3. 标识符的命名规范

1. 以字母或者下划线开头,后接字母、数字、下划线
2. 以操作符开头，且只包含操作符（+、-、*、/、#、！）
3. 用反引号包括的任意字符串，即使是Scala关键字也可以

## 4. 字符串输出

1. 基本语法

   （1）字符串， 通过 + 连接

   ```scala
   val name = "alice"
   val age = 20
   //* 用于一个字符串复制多次并连接
   println(name*3)  //输出三次name值
   //alicealicealice
   ```

   （2）printf用法： 字符串，通过%传值

   ```scala
   printf("%d岁的%s在尚硅谷学习", age, name)
   //20岁的alice在尚硅谷学习
   ```

   （3）字符串模板（插值字符串）：通过$获取变量值

   ```scala
   prinln(s"${age}岁的${name}在尚硅谷学习 ")
   //20岁的alice在尚硅谷学习
   
   val num: Double = 2.3456
   println(f"The num is ${num}%2.2f")	//格式化模板字符串
   //The num is 2.35
   println(raw"The num id ${num}%2.2f")
   //The num is 2.3456%2.2f
   
   //三引号表示字符串,保持多行字符串的原格式输出
   val sql = s"""
        |select *
        |from student
        |where
        |  name = ${name}
        |and
        |  age > ${age}
        |""".stripMargin
   println(sql)
   //select *
   //from student
   //where
   //  name = alice
   //and
   //  age > 20
   ```



## 5. 键盘输入

基本语法:

StdIn.readLine()

StdIn.readShort()

StdIn.reaDouble()

```scala
import scala.io.StdIn

object Test05_StdIn {
  def main(args: Array[String]): Unit = {
    println("请输入你的大名：")
    val name=StdIn.readLine()
    println("请输入你的年龄")
    val age=StdIn.readInt()
    println(s"欢迎${age}岁的${name}来到尚硅谷学习")
  }
}
```

文件输入

```scala
import java.io.{File, PrintWriter}

import scala.io.Source

object Test06_FileIO {
  def main(args: Array[String]): Unit = {
    //1. 从文件中读取数据
    Source.fromFile("E:\\Program Files (x86)\\BigData\\atguigu-classes\\scala\\src\\main\\resources\\test.txt").foreach(print)
    //2. 将数据写入文件
    val writer = new PrintWriter(new File("E:\\Program Files (x86)\\BigData\\atguigu-classes\\scala\\src\\main\\resources\\output.txt"))
    writer.write("hello scala from java writer")
    writer.close()
  }

}
```

## 6. 数据类型

1. Scala中一切数据都是对象,<font color="red">都是Any的子类</font>
2. Scala中数据类型分为两大类: <font color="red">数值类型(AnyVal)</font>、<font color="red">引用类型(AnyRef)</font>,不管是值类型还是引用类型都是对象
3. Scala数据类型仍然遵守，低精度的值类型向高精度值类型自动转换（隐式转换）
4. Scala中的String Ops时对Java中的String增强
5. Unit ：对应Java中的void，用于方法返回值的位置，表示方法没有返回值。Unit是一个数据类型，只有一个对象，就是().Void不是数据类型,只是一个关键字
6. Null是一个类型,只有一个对象就是null. 它是所有引用类型的子类
7. Nothing是所有数据类型的子类,主要用在一个函数没有明确返回值时使用,因为这样可以把抛出的返回值返回给任何的变量或者函数

![image-20220906180131908](Scala image\image-20220906180131908.png)  

## 7. 整数类型(Byte、Short、Int、Long)

| 数据类型 | 描述                                       |
| -------- | ------------------------------------------ |
| Byte[1]  | 8位有符号补码整数,数值区间为-128到127      |
| Short[2] | 16位有符号补码整数,数值区间为-32768到32767 |
| Int[4]   | 32位有符号补码整数                         |
| Long[8]  | 64位有符号补码整数                         |





# 运算符

## 1. 算术运算符

+-*/% 

## 2. 关系运算符

Scala中字符串的比较不管内存地址是否相同,都可以使用"=="判断内容是否相同,若要比较内存地址是否相同,则需使用eq方法

```scala
val s1: String = "hello"
val s2: String = new String("hello")

println(s1 == s2)	//true
println(s1.equals(s2))	//true
println(s1.eq(s2))	//false
```

## 3. 逻辑运算符

&&: 逻辑与

||: 逻辑或

!: 逻辑非 



# 流程控制

## 1. 分支控制 if-else

整个分支语句可以作为一个对象,将最终结果的分支的最后一句作为赋值的结果

```Scala
score = 60
val result = if(score<60){
	"不及格"
}else if(score<80){
    "及格"
}else if(score<=100){
    "优秀"
}
```

## 2. 循环控制

### 1. for循环

1. 范围数据循环(to和until)

   ```scala
   object Test02_ForLoop {
     def main(args: Array[String]): Unit = {
       // 1. 范围遍历(包含边界)
       for (i <- 0 to 9){
         println(i + ".hello world")
       }
       for (i <- 1.to(10)){
         println(i + ".hello world")
       }
       println("=============================")
       //(不包含边界)
   //    for (i <- Range(1, 10)){
   //      println(i + ".hello scala")
   //    }
       for (i <- 1 until 10){
         println(i + ".hello scala")
       }
       println("============================")
       //2. 集合遍历
       for(i <- Array(123, 34, 52)){
         println(i)
       }
       for (i <- List(21, 342, 43)){
         println(i)
       }
       println("===========================")
       //3. 循环守卫
       for (i <- 1 to 10 if i != 5){
         println(i)
       }
       println("============================")
       //4. 循环步长
       for (i <- 1 to 10 by 2){
         println(i)
       }
       println("============================")
       //5. 嵌套循环
       for (i <- 1 to 5){
         for (j <- 1 to 5){
           println("i = " + i +", j = " + j)
         }
       }
       for (i <- 1 to 5; j <- 1 to 5){
         println("i = " + i +", j = " + j)
       }
       for (i <- 1 to 9; j <- 1 to i){
   //      print(i + "*" + j + "=" + i*j + "\t")
         print(s"$j * $i = ${i*j} \t")
         if (i==j) println()
       }
     }
   }
   ```

2. 循环返回值

   > 基本语法:
   >
   > val res = for (i <- 1 to 10) yield i
   >
   > println(res)
   >
   > 返回值是一个Victory

   

### 2. while和do while循环

用法与Java相同



### 3. 循环中断

```scala
import scala.util.control.Breaks

object Test05_Break {
  def main(args: Array[String]): Unit = {
    //1. 采用抛出异常的方式,退出循环
    try {
      for(i <- 0 until 5){
        if(i==3){
          throw new RuntimeException
        }
        println(i)
      }
    }catch {
      case e: Exception =>  //什么都不做,只是退出循环
    }
  }

  //2. 使用Scala中的Breaks类的break方法,实现异常的抛出和捕捉
  Breaks.breakable(
    for (i <- 0 until 5){
      if (i==3){
        Breaks.break()
      }
      println(i)
    }
  )
}
```



# 函数式编程

1. 面向对象编程

   解决问题,分解对象,行为,属性,然后通过对象的关系以及行为的调用来解决问题

   对象: 用户

   行为: 登录, 连接JDBC, 读取数据库

   属性: 用户名,密码

   <font color="red">Scala语言是一个完全面向对象的语言,万物皆对象</font>

   对象的本质: 对数据的行为的一个封装

2. 函数式编程

   解决问题时,将问题一个一个

3. 函数和方法的区别

   (1) 为完成某一功能的程序语句的集合,称为函数

   (2) 类中的函数称之为方法

   ```scala
   object Test01_FunctionAndMethod {
     def main(args: Array[String]): Unit = {
       //定义函数
       def seyHi(name: String): Unit = {
         println("hi," + name)
       }
       //调用函数
       seyHi("alice")
       //调用对象的方法
       Test01_FunctionAndMethod.sayHi("bob")
     }
   
     //定义对象的方法
     def sayHi(name: String): Unit = {
       println("Hi," + name)
     }
   }
   ```

   

## 函数参数

```scala
object Test02_FunctionParameter {
  def main(args: Array[String]): Unit = {
    //1. 可变参数
    def f1(str: String*): Unit = {
      println(str)
    }
    f1("alice")    //WrappedArray(alice)
    f1("aaa", "bbb", "ccc")	//WrappedArray(aaa, bbb, ccc)

    //2. 如果参数列表中存在多个参数,快慢可变参数一般放置在最后
    def f2(str1: String, str2: String*): Unit = {
      println("str1:" + str1 + " str2:" + str2)
    }
    f2("alice")		//str1:alice str2:WrappedArray()
    f2("aaa", "bbb", "ccc")		//str1:aaa str2:WrappedArray(bbb, ccc)

    //3. 参数默认值,一般将有默认值的参数防止在参数列表的后面
    def f3(name: String = "atguigu"): Unit = {
      println("My school is "+ name)
    }
    f3("school")	//My school is school
    f3()	//My school is atguigu

    //4. 带名参数
    def f4(name: String = "atguigu", age: Int): Unit = {
      println(s"${age}岁的${name}在尚硅谷学习")
    }
    f4("alice", 20)		//20岁的alice在尚硅谷学习
    f4(age = 32, name = "bob")		//32岁的bob在尚硅谷学习
    f4(age = 31)	//31岁的atguigu在尚硅谷学习
  }
}
```

## 函数至简原则

能省则省

```Scala
object Test03_Simplify {
  def main(args: Array[String]): Unit = {
    def f0(name: String): String = {
      return name
    }
    println(f0("atguigu"))
    //1. return可以省略,Scala会使用函数体的最后一行代码作为返回值
    def f1(name: String): String = {
      name
    }
    println(f1("atguigu"))
    //2. 如果函数体只有一行代码,可以省略花括号
    def f2(name: String): String = name
    println(f2("atguigu"))
    //3. 返回值类型如果能够推断出来,那么可以省略(:和返回值类型一起省略)
    def f3(name: String) = name
    println(f3("atguigu"))
    //4. 如果有return,则不能省略返回值类型,必须指定
    def f4(name: String): String = {
      return name
    }
    println(f4("atguigu"))
    //5. 如果函数声明unit,那么即使函数体中使用return关键字也不起作用
    def f5(name: String): Unit = {
    }
    println(f5("atguigu"))
    //6. Scala如果所期望是无返回值类型,可以省略等号
    def f6(name: String) {
      println(name)
    }
    println(f6("atguigu"))
    //8. 如果函数无参,但是声明了参数列表,那么调用时,小括号,可加可不加
    def f7(): Unit = {
      println("atguigu")
    }
    f7()
    f7
    //7. 如果函数没有参数列表,那么小括号可以省略,调用时小括号必须省略
    def f8: Unit = {
      println("atguigu")
    }
    //f8()
    f8
    //9.如果不惯性名称,只关心逻辑处理,那么函数名(def)可以省略
//    def f9(name: String):Unit = {
//      println(name)
//    }
    //匿名函数,lambda表达式
    (name: String) => {println(name)}
  }
}
```

## 匿名函数

```Scala
object Test05_Lambda {
  def main(args: Array[String]): Unit = {
    val fun = (name: String) => {println(name)}
    fun("atguigu")

    println("======================")

    //定义一个函数,以函数作为参数输入
    def f(func: String => Unit): Unit = {
      func("chenlan")
    }
    f(fun)
    f((name: String) => {println(name)})

    println("======================")
    //匿名函数的简化原则
    //1. 参数的类型可以省略,会根据形参进行自动推导
    f((name) => {println(name)})
    //2. 参数省略之后,发现只有一个参数,则圆括号可以省略; 其他情况:没有参数和参数超过1的则不能省略圆括号
    f(name => {println(name)})
    //3. 匿名函数如果只有一行,则大括号也可以省略
    f(name => println(name))
    //4. 如果参数只出现一次,则参数省略且后面擦拭农户可以用_代替
    f( println(_))
    //5. 如果可以推断出当前传入的println是一个函数体,而不是调用语句,可以直接省略下划线
    f( println )
  }
}
```



## 高阶函数

```scala
object Test06_HighOrderFunction {
  def main(args: Array[String]): Unit = {
    def f(n: Int): Int = {
      println("f调用")
      n + 1
    }
    val result = f(123)
    println(result)   //f调用
                      //124
    def fun(): Int = {
      println("fun调用")
      2
    }
    //1. 函数作为值进行传递
    val f1: Int => Int = f
    val f2 = f _
    println(f1)   //chapter05.Test06_HighOrderFunction$$$Lambda$5/1057941451@7d417077
    println(f1(12))   //f调用
                      //13
    println(f2)   //chapter05.Test06_HighOrderFunction$$$Lambda$6/1975358023@7dc36524
    println(f2(23))   //f调用
                      //24

    val f3 = fun()
    val f4 = fun _
    println(f3)   //fun调用
                  //2
    println(f4)   //chapter05.Test06_HighOrderFunction$$$Lambda$7/1456208737@1134affc

    //2. 函数作为参数进行传递
    //定义一个二元计算函数
    def dualEval(op: (Int, Int)=> Int, a:Int, b: Int): Int = {
      op(a, b)
    }
    def add(a: Int, b: Int): Int = {
      a+b
    }
    println(dualEval(add, 12, 34))    //46
    println(dualEval((a, b)=> a + b, 12, 34))   //46
    println(dualEval(_ + _, 12, 34))    //46

    //3. 函数作为函数的返回值返回
    def f5(): Int=> Unit= {
      def f6(a: Int): Unit = {
        println("f6调用" + a)
      }
      f6    //将函数直接返回
    }
    println(f5())   //chapter05.Test06_HighOrderFunction$$$Lambda$11/1277181601@27f674d
    println(f5()(23))   //f6调用23
                        //()
  }
}
```

```Scala
object Test07_Practice_CollectionOperation {
  def main(args: Array[String]): Unit = {
      //对数组进行处理,将操作抽象出来,处理完毕之后的结果返回一个新的数组
    val arr: Array[Int] = Array(12, 34, 73, 23)
    def arrayOperation(array: Array[Int], op: Int=>Int): Array[Int] = {
      for(elem <- array) yield op(elem)
    }

    //定义一个加一操作
    def addOne(elem: Int): Int = {
      elem + 1
    }
    //调用函数
    val newArray: Array[Int] = arrayOperation(arr, addOne)
    println(newArray.mkString(","))		//13,35,74,24

    //传入匿名函数,实现元素翻倍
    val newArray2 = arrayOperation(arr, _ * 2)
    println(newArray2.mkString(","))		//24,68,146,46
  }

}
```

练习

```scala
object Test08_Practice {
  def main(args: Array[String]): Unit = {
    //定义一个匿名函数,并将它作为值赋给变量fun,函数有三个参数,类型分别为Int,String,Char,返回值类型为Boolean
    //要求调用函数fun(0,"",'0')得到返回值false,其他情况均返回true
    val fun = (a: Int, b: String, c: Char) => { if (a == 0 && b == "" && c =='0') false else true }
    println(fun(0, "", '0'))    //false
    println(fun(0, "", '2'))    //true
    println(fun(0, "df", '2'))    //true
  }

}
```

```scala
object Test09_Practice2 {
  def main(args: Array[String]): Unit = {
    //定义一个函数func,他接收一个Int类型的参数,返回一个函数(记作f1).
    // 他返回的函数f1,接收一个String类型的参数,同样返回一个函数(记作f2).函数f2接收一个Char类型的参数,返回一个Boolean的值
    //要求调用函数func(0)("")('0')得到返回值false,其他情况均返回true
    def func(n: Int): String => (Char=>Boolean) ={
      def f1(s: String): Char => Boolean ={
        def f2(c: Char): Boolean = {
          if(n == 0 && s == "" && c == '0')
            return false
          true
        }
        f2
      }
      f1
    }
    println(func(0)("")('0'))   //false
    println(func(0)("")('3'))   //true
    println(func(23)("afq")('3'))   //true

    //匿名函数简写
    def func2(n: Int): String => (Char=>Boolean) ={
      s => c => if(n == 0 && s == "" && c == '0') false else true
    }
    println(func2(0)("")('0'))   //false

    //柯里化
    def func3(n: Int)(s: String)(c: Char): Boolean = {
      if(n == 0 && s == "" && c == '0') false else true
    }
    println(func3(0)("")('0'))   //false
  }

}
```

## 函数柯里化&闭包

闭包: 如果一个函数,访问到了它的外部(局部)变量的值,那么这个函数和它所处的环境,称为闭包

函数柯里化: 把一个参数列表的多个参数,变成多个参数列表

```scala
object Test10_ClosureAndCurrying {
  def main(args: Array[String]): Unit = {
    def add(a: Int, b: Int): Int = {
      a+b
    }

    //1. 考虑固定一个加数的场景
    def addByFour(b: Int): Int = {
      4 + b
    }
    //2. 扩展固定家属改变的情况
    def addByFive(b: Int): Int = {
      5 + b
    }
    //3. 将固定加数作为另一个参数传入,但是是作为"第一层参数"传入
    def addByFour1(): Int=>Int = {
      val a = 4
      def addB(b: Int): Int = {
        a + b
      }
      addB
    }
    def addByA(a: Int): Int=>Int = {
      def addB(b: Int): Int = {
        a + b
      }
      addB
    }
    println(addByA(23)(34))

    val addByFour2 = addByA(4)
    val addByFive2 = addByA(5)
    println(addByFour2(13))
    println(addByFive2(25))

    //lambda表达式简写
    def addByA1(a: Int): Int=>Int = a + _
    val addByFour3 = addByA(4)
    val addByFive3 = addByA(5)
    println(addByFour3(13))
    println(addByFive3(25))
    //柯里化
    def addByA2(a: Int)(b: Int): Int = a+b
    println(addByA2(34)(37))
  }

}
```

## 递归

```Scala
object Test11_Recursion {
  def main(args: Array[String]): Unit = {
    println(fact(5))
  }
  //递归实现计算阶乘
  def fact(n: Int): Int = {
    if (n == 0) return 1
    fact(n - 1) * n
  }
  //尾递归
  def tailFact(n: Int): Int = {
    @scala.annotation.tailrec
    def loop(n: Int, currRes: Int): Int = {
      if (n==0) return currRes
      loop(n - 1, currRes * n)
    }
    loop(n, 1)
  }

}
```

## 控制抽象

```scala
object Test12_ControlAbstraction {
  def main(args: Array[String]): Unit = {
    //1. 传值参数
    def f0(a: Int): Unit = {
      println("a:" + a)
      println("a:" + a)
    }
    f0(23)
    def f1(): Int = {
      println("f1调用")
      12
    }
    f0(f1())

    println("==========================")
    //2. 传名参数,传递的不再是具体的值,二十代码块
    def f2(a: =>Int): Unit = {
      println("a:" + a)
      println("a:" + a)
    }
    f2(23)
    f2(f1())
    f2({
      println("这是一个代码块")
      34
    })
  }
}
```

> a:23
> a:23
> f1调用
> a:12
> a:12
> 
> ===========================
> a:23
> a:23
> f1调用
> a:12
> f1调用
> a:12
> 这是一个代码块
> a:34
> 这是一个代码块
> a:34

实例: 写一个能实现while功能的代码

```scala
object Test13_MyWhile {
  def main(args: Array[String]): Unit = {
    var n = 10
    //1. 常规的while循环
    while (n >= 1){
      println(n)
      n -= 1
    }
    println("=============================")
    //2. 用闭包实现一个函数,将代码块作为参数传入,递归调用
    def myWhile(condition: =>Boolean): (=>Unit)=>Unit ={
      //内层函数需要递归调用,参数就是循环体
      def doLoop(op: =>Unit): Unit = {
        if(condition){
          op
          myWhile(condition)(op)
        }
      }
      doLoop _
    }

    n = 10
    myWhile(n >= 1){
      println(n)
      n -= 1
    }
    println("=============================")

    //3. 用匿名函数实现
    def myWhile2(condition: =>Boolean): (=>Unit)=>Unit ={
      //内层函数需要递归调用,参数就是循环体
      op => {
        if(condition){
          op
          myWhile2(condition)(op)
        }
      }
    }
    n = 10
    myWhile2(n >= 1){
      println(n)
      n -= 1
    }
    println("=============================")
    //4. 用柯里化实现
    def myWhile3(condition: =>Boolean)(op: =>Unit): Unit = {
      if(condition){
        op
        myWhile3(condition)(op)
      }
    }
    n = 10
    myWhile2(n >= 1){
      println(n)
      n -= 1
    }
  }
}
```

## 惰性加载

当函数返回值被声明为lazy时,函数的执行将被推迟,直到我们首次对此取值,该函数才会执行

```scala
object Test14_Lazy {
  def main(args: Array[String]): Unit = {
    lazy val result: Int = sum(13, 47)
    println("1. 函数调用")
    println("2. result = " + result)
    println("4. result = " + result)
  }
  def sum(i: Int, j: Int): Int = {
    println("3. sum调用")
    i + j
  }

}
```

> 1. 函数调用
> 3. sum调用
> 2. result = 60
> 4. result = 60

sum只会调用一次



# 面向对象

## 1. Scala包

包命名规范: com.公司名称.项目名.业务模块名

Scala有两种包的管理风格,一种方式和Java的包管理风格相同,每个源文件一个包(包名和源文件所在路径不要求必须一致), 包名用"."进行分隔以表示包的层级关系, 如com.atguigu.Scala

另一种风格,通过嵌套的风格表示层级关系:

> package com{
>
> ​	package atguigu{
>
> ​		package scala{
>
> ​		}
>
> ​	}
>
> }

此风格特点:

1. 一个源文件中可以声明多个package
2. 子包中的类可以直接访问父包中的内容,而无需导包

**包对象**

在Scala中可以为每个包定义一个同名的包对象,定义在包对象中的成员,作为其对应包下所有class和object的共享变量,可以被直接访问

**导包说明**

1. 和Java一样,可以在顶部使用import导入,在这个文件中的所有类都可以使用
2. 局部导入: 什么时候使用,什么时候导入. 在起作用范围内都可以使用
3. 通配符导入: ```import java.util._```
4. 给类起别名: ```import java.util.{ArrayList=>JL}```
5. 导入相同包的多个类: ```import java.util.{HashSet, ArrayList}```
6. 屏蔽类: ```import java.util.{ArrayList=>_,_}```
7. 导入包的绝对路径 ```new _root_.java.util.HashMap```

<font color="red">注意:</font>

Scala中默认三个导入

import java.lang._

import scala._

import scala.Predef._

## 2. 类和对象

类: 可以看成一个模板

对象: 表示具体的事物

Scala中没有public,所有这些类都具有共有可见性, 一个.scala文件中可以写多个类

```scala
import scala.beans.BeanProperty

object Test03_Class {
  def main(args: Array[String]): Unit = {
    //创建一个对象
    val student = new Student()
//    student.name    //error, 不能访问private属性
    println(student.age)
    println(student.sex)
    student.sex = "female"
    println(student.sex)
  }

}

//定义一个类
class Student {
  //定义属性
  private var name: String = "alice"
  @BeanProperty
  var age: Int = _
  var sex: String = _
}
```

> 0
> null
> female

## 3. 封装

封装就是把抽象的数据和对数据的操作封装在一起,数据被保护在内部,程序的其他部分只有通过被授权的操作(成员方法), 才能对数据进行操作

Scala不推荐将属性设为private,再为其设置public的get和set方法的做法.所以一般通过==@BeanProperty注解==实现



**访问权限**

1. Scala中属性和方法的默认访问权限为public,但Scala中没有public关键字
2. private为四有权限,只有类内部和伴生对象中可用
3. protected为受保护权限,Scala中受保护权限比Java中更严格,同类,子类可以访问,同包无法访问
4. private[包名]增加包访问权限, 包名下的其他类也可以使用

```Scala
object Test04_Access {
  def main(args: Array[String]): Unit = {
    //创建对象
    val person: Person = new Person()
    println(person.age)
    println(person.sex)
    person.printInfo()

    val worker: Worker = new Worker()
    worker.printInfo()
  }

}

//定义一个子类
class Worker extends Person {
  override def printInfo(): Unit = {
    println("Worker: ")
    name = "bob"
    age = 24
    sex = "male"
    println(s"Worker: ${name} ${age} ${sex}")
  }
}
```

```scala
object Test04_ClassForAccess {

}

//定义一个父类
class Person {
  private var idCord: String = "234222"
  protected var name: String = "alice"
  var sex: String = "female"
  private[chapter06] var age: Int = 18

  def printInfo(): Unit = {
    println(s"Person: $idCord $name $sex $age")
  }
}
```

**构造器**

基本语法:

> class 类名(形参列表){	//主构造器
>
> ​	//类体
>
> ​	def this(形参列表){	//辅助构造器
>
> ​	}
>
> }

说明:

1. 辅助构造器,函数名称this,可以有多个,通过参数的个数及类型区分
2. 辅助构造方法不能直接构造的对象, 必须直接或间接调用主构造方法
3. 构造器调用其他另外的构造器,要求被调用构造器必须提前声明

```scala
object Test05_Constructor {
  def main(args: Array[String]): Unit = {
    val student1 = new Student1
    student1.Student1()
    println("===========================")
    val student2 = new Student1("alice")
    println("===========================")
    val student3 = new Student1("bob", 23)
  }
}

//定义一个类
class Student1 {
  //定义属性
  var name: String = _
  var age: Int = _
  println("1. 主构造器方法被调用")

  //声明夫主构造方法
  def this(name: String) {
    this()
    println("2. 辅助构造方法被1调用")
    this.name = name
    println(s"name: $name age: $age")
  }

  def this(name: String, age: Int) {
    this(name)
    println("3.辅助构造方法2被调用")
    this.age = age
    println(s"name: $name age: $age")
  }

  def Student1(): Unit = {
    println("一般方法被调用")
  }
}
```

> 1.主构造器方法被调用
> 一般方法被调用
>
> ===========================
>
> 1.主构造器方法被调用
>
> 2.辅助构造方法被1调用
> name: alice age: 0
>
> ===========================
>
> 1.主构造器方法被调用
>
> 2.辅助构造方法被1调用
> name: bob age: 0
> 3.辅助构造方法2被调用
> name: bob age: 23

**构造器参数**

1. 不使用任何修饰符修饰, 这个参数为局部变量
2. var修饰参数,作为类的成员属性使用,可以修改
3. val修饰参数,作为类只读属性使用,不能修改

```scala
object Test06_ConstructorParams {
  def main(args: Array[String]): Unit = {
    val student2 = new Student2
    student2.name = "alice"
    student2.age = 23
    println(s"student2: name = ${student2.name}, age = ${student2.age}")

    val student3 = new Student3("bob", 23)
    println(s"student3: name = ${student3.name}, age = ${student3.age}")
    val student4 = new Student4("cary", 23)
    student4.printInfo()

    val student6 = new Student5("david", 35, "atguigu")
    println(s"student6: name = ${student6.name}, age = ${student6.age}")
    student6.printInfo()
  }
}

//定义类
//无参构造器
class Student2 {
  //单独定义属性
  var name: String = _
  var age: Int = _
}
//上面定义等价于
class Student3(var name: String, var age: Int)

//主构造器参数无修饰
class Student4(name: String, age: Int){
  def printInfo(): Unit ={
    println(s"student4: name = $name, age = $age")
  }
}

class Student5(val name:String, var age: Int){
  var school: String = _

  def this(name: String, age: Int, school: String){
    this(name, age)
    this.school = school
  }

  def printInfo(): Unit ={
    println(s"student5: name = $name, age = $age, school = $school")
  }
}
```

> student2: name = alice, age = 23
> student3: name = bob, age = 23
> student4: name = cary, age = 23
> student6: name = david, age = 35
> student5: name = david, age = 35, school = atguigu



## 4. 继承和多态

```class 子类名 extends 父类名 {类体}```

```scala
object Test07_Inherit {
  def main(args: Array[String]): Unit = {
    val student1 = new Student7("alice", 23)
    val student2 = new Student7("bob", 12, "std001")
  }

}

//定义一个父类
class Person7(){
  var name: String = _
  var age: Int = _

  println("1. 父类的主构造器调用")

  def this(name: String, age: Int){
    this()
    println("2. 父类的辅助构造器调用")
    this.name = name
    this.age = age
  }

  def printInfo(): Unit ={
    println(s"Person: $name $age")
  }
}

//定义子类
class Student7(name: String, age: Int) extends Person7() {
  var stdNo: String = _

  println("3. 子类的主构造器被调用")

  def this(name: String, age: Int, stdNo: String){
    this(name, age)
    println("4. 子类的辅助构造器调用")
    this.stdNo = stdNo
  }

  override def printInfo(): Unit = {
    println(s"Student: $name $age $stdNo")
  }
}
```

> 1. 父类的主构造器调用
> 3. 子类的主构造器被调用
> 1. 父类的主构造器调用
> 3. 子类的主构造器被调用
> 4. 子类的辅助构造器调用

```Scala
object Test08_DynamicBind {
  def main(args: Array[String]): Unit = {
    val student: Person8 = new Student8
    println(student.name)
    student.hello()
  }

}

class Person8{
  val name:String = "person"
  def hello(): Unit = {
    println("hello person")
  }
}
class Student8 extends Person8{
  override val name: String  = "student"
  override def hello(): Unit = {
    println("hello student")
  }
}
```

> student
> hello student

==属性和方法都是动态绑定的==

## 5. 抽象类

### 1. 抽象属性和抽象方法

1. 抽象类: abstract关键字标记抽象类
2. 抽象属性: 没有初始值的属性
3. 抽象方法: 只声明没有实现的方法

```scala
object Test09_AbstractClass {
  def main(args: Array[String]): Unit = {
    val student = new Student9
    student.sleep()
    student.eat()
  }

}

//定义抽象类
abstract class Person9{
  //非抽象属性
  val name: String = "person"
  //抽象属性
  var age: Int
  //非抽象方法
  def eat(): Unit = {
    println("person eat")
  }
  //抽象方法
  def sleep(): Unit
}

//帝国一具体的实现子类
class Student9 extends Person9 {
  //实现抽象属性和方法
  var age: Int = 18
  def sleep(): Unit =  {
    println("student sleep")
  }

  //重写非抽象属性和方法
  override val name: String = "student"

  override def eat(): Unit = {
    super.eat()
    println("student eat")
  }
}
```

> student sleep
> person eat
> student eat

### 2. 匿名子类

和Java一样, 可以通过包含带有定义或重写的代码块的方式创建一个匿名的子类

```scala
object Test10_AnnoymousClass {
  def main(args: Array[String]): Unit = {
    val person: Person10 = new Person10 {
      override var name: String = "alice"

      override def eat(): Unit = println("person eat")
    }
    println(person.name)
    println(person.eat())
  }

}

//定义抽象类
abstract class Person10{
  var name: String
  def eat(): Unit
}
```

> alice
> person eat
> ()



## 6. 单例对象(伴生对象)

Scala语言是完全面对对象的语言,没有静态的操作,为了能与Java语言交互,就产生了一种特殊的对象来模拟类对象,该对象为单例对象. 若单例对象与类名一致,则称该单例对象这个类的伴生对象

说明:

1. 单例对象采用object关键字声明
2. 单例对象对应的类称之为伴生类,伴生对象的名称应该和伴生类名一致
3. 单例对象中的属性和方法都可以通过伴生对象名直接调用访问

```scala
object Test11_Object {
  def main(args: Array[String]): Unit = {
//    val student = new Student11("alice", 19)
//    student.printInfo()

    val student = Student11.newStudent("alice", 34)
    student.printInfo()

    val student2 = Student11.apply("bob", 19)
    student2.printInfo()
    //apply 可以省略
    val student3 = Student11("bob", 19)
    student3.printInfo()
  }

}

//定义类         构造方法私有化
class Student11 private(val name: String, val age: Int){
  def printInfo(): Unit ={
    println(s"student: name = $name, age = $age, school = ${Student11.school}")
  }
}

//类的伴生对象
object Student11{
  val school: String = "atguigu"
  //定义一个类的对象实例的创建方法
  def newStudent(name: String, age: Int): Student11 = new Student11(name, age)

  def apply(name: String, age: Int): Student11 = new Student11(name, age)
}
```

> student: name = alice, age = 34, school = atguigu
> student: name = bob, age = 19, school = atguigu
> student: name = bob, age = 19, school = atguigu



```scala
//单例设计模式
object Test12_Singleton {

  def main(args: Array[String]): Unit = {
    val student1 = Student12.getInstance()
    student1.printInfo()

    val student2 = Student12.getInstance()
    student2.printInfo()

    println(student1)
    println(student2)
  }
}

//定义类         构造方法私有化
class Student12 private(val name: String, val age: Int){
  def printInfo(): Unit ={
    println(s"student: name = $name, age = $age, school = ${Student11.school}")
  }
}

//饿汉式
//object Student12 {
//  private val student: Student12 = new Student12("alice", 23)
//  def getInstance(): Student12 = student
//}

//懒汉式
object Student12 {
  private var student: Student12 = _
  def getInstance(): Student12 = {
    //如果没有对象实例的话,就创建一个
    if(student == null){
      student = new Student12("alice", 19)
    }
    student
  }
}
```

> student: name = alice, age = 19, school = atguigu
> student: name = alice, age = 19, school = atguigu
> chapter06.Student12@66d33a
> chapter06.Student12@66d33a

创建的两个常量的值是一摸一样的

## 7. 特质

Scala语言中,采用特质trait(特征)来代替接口的概念, 即多个类具有相同的特质时, 就可以将这个特质独立出来, 采用关键字```trait```声明

trait中**<font color="red">即可以有抽象属性和方法,可以有具体的属性和方法,一个类可以混入多个特质</font>**

### 1. 特质声明

基本语法

> trait 特质名{
>
> ​	trait主体
>
> }

一个类具有某种特质, 就意味着这个类满足了这个特质的所有要素,所以在使用时,也采用了```extends```关键字,如果存在多个特质或存在父类,则需用```with```关键字连接

基本语法

> 没有父类: class 类名 extends 特质1 with 特质2
>
> 有父类: class 类名 extends 父类 with 特质1 with 特质2

说明:

1. 类和特质的关系: 使用和继承的关系
2. 当一个类去继承特质时, 第一个连接词时extends,后面是with

```scala
object Test13_Trait {
  def main(args: Array[String]): Unit = {
    val student = new Student13
    student.sayHello()
    student.study()
    student.dating()
    student.play()
  }

}

//定义一个父类
class Person13{
  val name: String = "person"
  var age: Int = 18
  def sayHello(): Unit = {
    println("hello from " + name)
  }
}

//定义一个特质
trait Young{
  // 声明抽象和非抽象属性
  var age: Int
  val name: String = "young"
  //声明抽象和非抽象方法
  def play(): Unit = {
    println(s"young people $name is playing")
  }
  def dating(): Unit
}

class Student13 extends Person13 with Young {
  //重写冲突属性
  override val name: String = "student"
  //实现抽象方法
  def dating(): Unit = println(s"student $name is dating")
  def study(): Unit = println(s"student $name is studying")

  //重写父类方法
  override def sayHello(): Unit = {
    super.sayHello()
    println(s"hello from : student $name")
  }
}
```

> hello from student
> hello from : student student
> student student is studying
> student student is dating
> young people student is playing

### 2. 动态混入

```scala
object Test14_TraitMixin {
  def main(args: Array[String]): Unit = {
    val student = new Student14
    student.study()
    student.increase()

    student.play()
    student.increase()

    student.dating()
    student.increase()

    println("===========================")
    //动态混入
    val studentWithTalen = new Student14 with Talent{
      override def dancing(): Unit = println("student is good at dancing")

      override def singing(): Unit = println("student is good at singing")
    }
    studentWithTalen.sayHello()
    studentWithTalen.play()
    studentWithTalen.study()
    studentWithTalen.dating()
    studentWithTalen.dancing()
    studentWithTalen.singing()
  }
}

//在定义一个特质
trait Knowledge {
  var amount: Int = 0
  def increase(): Unit
}

trait Talent {
  def singing(): Unit
  def dancing(): Unit
}

class Student14 extends Person13 with Young with Knowledge {
  //重写冲突属性
  override val name: String = "student"
  //实现抽象方法
  def dating(): Unit = println(s"student $name is dating")
  def study(): Unit = println(s"student $name is studying")

  //重写父类方法
  override def sayHello(): Unit = {
    super.sayHello()
    println(s"hello from : student $name")
  }

  //实现特质中的抽象方法
  override def increase(): Unit = {
    amount += 1
    println(s"student $name knowledge increased: $amount")
  }
}
```

> student student is studying
> student student knowledge increased: 1
> young people student is playing
> student student knowledge increased: 2
> student student is dating
> student student knowledge increased: 3
> 
> ============================
> hello from student
> hello from : student student
> young people student is playing
> student student is studying
> student student is dating
> student is good at dancing



```scala
object Test15_TraitOverlying {
  def main(args: Array[String]): Unit = {
    val student = new Student15()
    student.increase()
  }
}

trait Knowledge15 {
  var amount: Int = 0
  def increase(): Unit = {
    println("knowledge increased")
  }
}

trait Talent15 {
  def singing(): Unit
  def dancing(): Unit
  def inscrease(): Unit = {
    println("knowledge increased")
  }
}

class Student15 extends Person13 with Talent15 with Knowledge15 {
  override def dancing(): Unit = println("dancing")

  override def singing(): Unit = println("singing")

  override def increase(): Unit = {super.increase()}
}
```

> knowledge increased

一个类继承了父类且混入了多个特质时, 且特质中有方法冲突, 可以重写方法, 所调用的super最后会输出后面的特质中的内容, 只有没有特质混入时, super调用的才是自己真正的父类



一个类混入的两个trait(TraitA, TraitB)中具有相同的具体方法,且两个trait继承自相同的trait(TraitC), 即所谓的"转砖石问题" , Scala采用了**特质叠加**策略, 所谓**特质叠加**, 就是将混入的多个trait中的冲突方叠加起来

```scala
object Test15_TraitOverlying {
  def main(args: Array[String]): Unit = {
    //砖石问题叠加
    val myFootBall = new MyFootBall
    println(myFootBall.describe())
  }
}

//定义球类
trait Ball {
  def describe(): String = "ball"
}

//定义颜色特征
trait ColorBall extends Ball {
  var color: String = "red"

  override def describe(): String = color + "-" + super.describe()
}

//定义种类特征
trait CategoryBAll extends Ball {
  var category: String = "foot"

  override def describe(): String = category + "-" + super.describe()
}

//定义一个自定义球的类
class MyFootBall extends CategoryBAll with ColorBall {
  override def describe(): String = "my ball is " + super.describe()
}
```

> my ball is red-foot-ball

特征的叠加顺序时从右到左

### 3. 特质和抽象类的区别

1. 优先使用特质. 一个类扩展多个特质是很方便的,但却只能扩展一个抽象类
2. 如果需要构造函数参数,使用抽象类, 因为抽象类可以定义带参数的构造函数,而特质不行

### 4. 特质的自身类型

自身类型可实现以来注入的功能

```scala
object Test16_TraitSelfType {
  def main(args: Array[String]): Unit = {
    val user = new RegisterUser("alice", "12344444")
    user.insert()
  }
}

//用户类
class User(val name: String, val password: String)

trait UserDao {
  _: User =>

  //向数据库插入数据
  def insert(): Unit = {
    println(s"insert into db: ${this.name}")
  }
}

//定义注册用户类
class RegisterUser(name: String, password: String) extends User(name, password) with UserDao
```

> insert into db: alice



## 8. 扩展

### 1. 类型检查和转换

1. ```obj.isInstanceOf[T]```: 判断obj是不是T类型
2. ```obj.asInstanceOf[T]```: 将obj强转成T类型
3. ```classOf```: 获取对象的类名

```scala
object Test17_Extends {
  def main(args: Array[String]): Unit = {
    //1. 类型的检测和转换
    val student: Student17 = new Student17("alice", 19)
    student.study()
    student.sayHi()
    val person: Person17 = new Student17("bob", 29)
    person.sayHi()

    println("student is Student17: " + student.isInstanceOf[Student17])
    println("student is Person17: " + student.isInstanceOf[Person17])
    println("person is Student17: " + person.isInstanceOf[Student17])
    println("person is Person17: " + person.isInstanceOf[Person17])

    val person2: Person17 = new Person17("cary", 23)
    println("person2 is Student17: " + person2.isInstanceOf[Student17])

    //类型转换
    if(person.isInstanceOf[Student17]){
      val newStudent = person.asInstanceOf[Student17]
      newStudent.study()
    }

    println(classOf[Student17])
  }

}

class Person17(val name:String, val age: Int){
  def sayHi(): Unit = {
    println("hi from person" + name)
  }
}

class Student17(name: String, age: Int) extends Person17(name, age ){
  override def sayHi(): Unit = {
    println("hi from student" + name)
  }
  def study(): Unit = {
    println("student study")
  }
}
```

> student study
> hi from studentalice
> hi from studentbob
> student is Student17: true
> student is Person17: true
> person is Student17: true
> person is Person17: true
> person2 is Student17: false
> student study
> class chapter06.Student17

### 2. 枚举类和应用类

枚举类: 继承 Enumeration

应用类: 继承App

```Scala
object Test17_Extends {
  def main(args: Array[String]): Unit = {

    //测试枚举类
    println(WorkDay.MONDAY)
  }

}


//定义枚举类对象
object WorkDay extends Enumeration {
  val MONDAY = Value(1, "Monday")
  val TUESDAY = Value(2, "Tuesday")
}

//定义应用类对象
object TestApp extends App {
  println("app start")

  type MyString = String
  val a: MyString = "abd"
  println(a)
}
```

> Monday

> app start
> abd

Type: 起别名



# 集合

## 1. 集合简介

1. Scala集合有三大类: 序列Seq, 集Set, 映射Map. 所有集合都扩展自Iterable特质

2. 对几乎所有的集合类,Scala都同时提供了**可变**和**不可变**的版本, 分别位于以下两个包:

   不可变集合: ```scala.collection.immutable```

   可变集合: ```scala.collection.mutable```

3. 不可变集合: 该集合对象不可修改, 每次修改就会返回一个新对象, 而不会对原对象进行修改

4. 可变集合: 该集合可以直接对原对象进行修改,而不会返回新的对象

**<font color="red">在操作集合的时候, 不可变用符号, 可变用方法</font>**

不可变集合图:

![Traversable](Spark%20image/Traversable.png)

## 2. 数组

### 1. 不可变数组

定义: ```val arr1 = new Array[Int](10)```

```scala
object Test01_ImmutableArray {
  def main(args: Array[String]): Unit = {
    //1. 创建数组
    val arr: Array[Int] = new Array[Int](5)
    //另一种创建方式
    val arr2 = Array(12, 34, 58, 23, 94)

    //2. 访问数组中的元素
    println(arr(0))
    println(arr(1))
    arr(0) = 14
    arr(1) = 23
    println(arr(0))
    println(arr(1))

    //3. 数组遍历
    //(1)普通for循环
    for(i <- 0 until arr.length) {
      println(arr(i))
    }
    for(i <- arr.indices){
      println(arr(i))
    }
    println("--------------------------")
    //(2) 直接遍历所有元素,增强for循环
    for (elem <- arr2) println(elem)
    println("--------------------------")
    //(3) 迭代器
    val iter = arr2.iterator
    while (iter.hasNext) {
      println(iter.next())
    }
    println("--------------------------")
    //4) 调用foreach循环
    arr2.foreach((elem: Int) => println(elem))
    arr.foreach( println )
    println(arr2.mkString("--"))
    println("--------------------------")

    //4. 添加元素
    //在数组后面添加数据
    val newArr = arr2.:+(73)
    println(arr2.mkString("--"))
    println(newArr.mkString("--"))

    //在数组后面添加数据
    val newArr2 = newArr.+:(30)
    println(newArr2.mkString("--"))

    val newArr3 = newArr2 :+ 15
    val newArr4 = 10 +: 10 +: newArr3 :+ 29
    println(newArr3.mkString("--"))
    println(newArr4.mkString(","))
  }

}
```

> 0
> 0
> 14
> 23
> 14
> 23
> 0
> 0
> 0
> 14
> 23
> 0
> 0
> 0
> 
> --------------------------
> 12
> 34
> 58
> 23
> 94
> 
> --------------------------
> 12
> 34
> 58
> 23
> 94
> 
> --------------------------
> 12
> 34
> 58
> 23
> 94
> 14
> 23
> 0
> 0
> 0
> 12--34--58--23--94
> 
> --------------------------
> 12--34--58--23--94
> 12--34--58--23--94--73
> 30--12--34--58--23--94--73
> 30--12--34--58--23--94--73--15
> 10,10,30,12,34,58,23,94,73,15,29

### 2. 可变数组

```scala
import scala.collection.mutable
import scala.collection.mutable.ArrayBuffer

object Test02_ArrayBuffer {
  def main(args: Array[String]): Unit = {
    //1. 创建可变数组
    val arr1 = new ArrayBuffer[Int]()
    val arr2 = ArrayBuffer(23, 32, 52)
    println(arr1)
    println(arr2)
    //2. 访问数组元素
//    println(arr1(0))   //error
    println(arr2(1))
    arr2(1) = 45
    //3. 添加元素
    //往后面添加数据
    arr1 += 19
    println(arr1)
    //往前面添加数据
    77 +=: arr1
    println(arr1)

    //直接调用方法
    arr1.append(29)
    arr1.prepend(12, 32)
    arr1.insert(1, 12, 32)  //第一个参数为添加元素的位置
    println(arr1)
    //添加一个数组的元素
    arr1.insertAll(2, arr2) //在指定的位置添加数组的元素
    arr1.appendAll(arr2)  //在末尾添加数组的元素

    //4. 删除元素
    arr1.remove(3)  //删除指定位置的元素
    println(arr1)
    arr1.remove(0, 10)  //连续删除从指定位置开始的count个数
    println(arr1)

    arr1 -= 13  //删除指定的元素
    println(arr1)

    //5. 可变数组转变为不可变数组
    val arr = ArrayBuffer(12, 34, 45)
    val newArr: Array[Int] = arr.toArray
    println(newArr.mkString("--"))
    println(arr)

    //6. 不可变数组转变为可变数组
    val buffer: mutable.Buffer[Int] = newArr.toBuffer
    println(buffer)
    println(newArr)
  }
}
```

> ArrayBuffer()
> ArrayBuffer(23, 32, 52)
> 32
> ArrayBuffer(19)
> ArrayBuffer(77, 19)
> ArrayBuffer(12, 12, 32, 32, 77, 19, 29)
> ArrayBuffer(12, 12, 23, 52, 32, 32, 77, 19, 29, 23, 45, 52)
> ArrayBuffer(45, 52)
> ArrayBuffer(45, 52)
> 12--34--45
> ArrayBuffer(12, 34, 45)
> ArrayBuffer(12, 34, 45)
> [I@3d8c7aca

### 3. 多维数组

```scala
object Test03_MulArray {
  def main(args: Array[String]): Unit = {
    //1. 创建二维数组
    val array: Array[Array[Int]] = Array.ofDim[Int](2, 3)
    //2. 访问元素
    array(0)(2) = 19
    array(1)(0) = 29
    println(array.mkString("--"))
    for(i <- array.indices; j <- array(i).indices){
      print(array(i)(j) + "\t")
      if (j == array(i).length - 1) println()
    }

    array.foreach(line => line.foreach(println))
    array.foreach(_.foreach(println))
  }

}
```

> [I@511baa65--[I@340f438e
> 0	0	19	
> 29	0	0	
> 0
> 0
> 19
> 29
> 0
> 0
> 0
> 0
> 19
> 29
> 0
> 0



## 3. 列表

### 1. 不可变列表

```scala
object Test04_List {
  def main(args: Array[String]): Unit = {
    //1. 创建一个列表
    val list1 = List(23, 43, 42)
    println(list1)
    //2. 访问和遍历元素
    println(list1(1))
    list1.foreach(println)
    //3. 添加元素
    val list2 = list1.+:(10)  //在前面添加数字
    val list3 = list1 :+ 23 //在后面添加元素
    println(list2)
    println(list3)
    println("-------------------------")

    val list4 = list2.::(43)  //在前面添加元素
    println(list4)
    val list5 = Nil.::(32)  //:: 主要用来创建一个新列表
    println(list5)
    val list6 = 32 :: Nil
    val list7 = 21 :: 89 :: 32 :: Nil
    println(list7)

    //将两个列表合并在一起
    val list8 = list6 :: list7
    println(list8)  //将list6这个列表放在了list7中
    val list9 = list6 ::: list7
    println(list9)  //将两个list中的元素合并在一起

    val list10 = list6 ++ list7
    println(list10) //与list9效果相同
  }

}
```

```
List(23, 43, 42)
43
23
43
42
List(10, 23, 43, 42)
List(23, 43, 42, 23)
-------------------------
List(43, 10, 23, 43, 42)
List(32)
List(21, 89, 32)
List(List(32), 21, 89, 32)
List(32, 21, 89, 32)
List(32, 21, 89, 32)
```



### 2. 可变列表

```scala
import scala.collection.mutable.ListBuffer

object Test05_ListBuffer {
  def main(args: Array[String]): Unit = {
    //1. 创建可变列表
    val list1: ListBuffer[Int] = new ListBuffer[Int]()
    val list2 = ListBuffer(12, 34, 54)
    println(list1)
    println(list2)
    println("----------------------")
    //2. 添加元素
    list1.append(21, 32)
    list2.prepend(38)
    list1. insert(1, 43, 21)
    println(list1)
    println(list2)
    92 +=: 83 +=: list1 += 25 += 30
    println(list1)
    println("----------------------")
    //3. 合并list
    val list3 = list1 ++ list2	//list1和list2列表没有变
    println(list1)
    println(list2)
    println(list3)
    println("----------------------")
    list1 ++= list2 //将list2中的元素添加到list1的后面
    println(list1)
    println(list2)
    println("----------------------")
    list1 ++=: list2  // 将list1中的元素添加到list2的前面
    println(list1)
    println(list2)
    println("----------------------")
    //4. 修改元素
    list2(2) = 83 //修改指定位置处的元素
    list2.update(1, 43) //修改指定位置处的元素
    println(list2)
    println("----------------------")
    //5. 删除元素
    list2.remove(2) //删除指定位置处的元素
    list2 -= 43 //删除指定元素
    println(list2)
  }
}
```

```
ListBuffer()
ListBuffer(12, 34, 54)
----------------------
ListBuffer(21, 43, 21, 32)
ListBuffer(38, 12, 34, 54)
ListBuffer(92, 83, 21, 43, 21, 32, 25, 30)
----------------------
ListBuffer(92, 83, 21, 43, 21, 32, 25, 30)
ListBuffer(38, 12, 34, 54)
ListBuffer(92, 83, 21, 43, 21, 32, 25, 30, 38, 12, 34, 54)
----------------------
ListBuffer(92, 83, 21, 43, 21, 32, 25, 30, 38, 12, 34, 54)
ListBuffer(38, 12, 34, 54)
----------------------
ListBuffer(92, 83, 21, 43, 21, 32, 25, 30, 38, 12, 34, 54)
ListBuffer(92, 83, 21, 43, 21, 32, 25, 30, 38, 12, 34, 54, 38, 12, 34, 54)
----------------------
ListBuffer(92, 43, 83, 43, 21, 32, 25, 30, 38, 12, 34, 54, 38, 12, 34, 54)
----------------------
ListBuffer(92, 43, 21, 32, 25, 30, 38, 12, 34, 54, 38, 12, 34, 54)
```



## 4. Set集合

默认情况下, scala引用的是不可变的set

### 1. 不可变set集合

```scala
object Test06_ImmutableSet {
  def main(args: Array[String]): Unit = {
    //1. 创建set
    val set1 = Set(34, 42, 53, 32, 4, 32)
    println(set1)
    println("-------------------------")
    //2. 添加元素
    //val set2 = set1 + 23
    val set2 = set1.+(23)
    println(set1)
    println(set2)
    println("-------------------------")
    //3. 合并set
    val set3 = Set(34, 23, 53, 81, 98)
    val set4 = set2 ++ set3
    println(set2)
    println(set3)
    println(set4)
    println("-------------------------")
    //4. 删除元素
    val set5 = set3 - 23
    println(set3)
    println(set5)
  }

}
```

```
Set(42, 53, 32, 34, 4)
-------------------------
Set(42, 53, 32, 34, 4)
Set(42, 53, 32, 34, 23, 4)
-------------------------
Set(42, 53, 32, 34, 23, 4)
Set(53, 34, 81, 98, 23)
Set(42, 53, 32, 34, 81, 98, 23, 4)
-------------------------
Set(53, 34, 81, 98, 23)
Set(53, 34, 81, 98)
```



### 2. 可变set集合

```scala
import scala.collection.mutable

object Test07_MutableSet {
  def main(args: Array[String]): Unit = {
    //1. 创建set
    val set1 = mutable.Set(13, 34, 33, 23, 42, 23)
    println(set1)
    println("------------------------")
    //2. 添加元素
    val set2 = set1 + 11
    println(set1)
    println(set2)

    set1 += 11
    println(set1)
    val flag1 = set1.add(10)
    println(flag1)
    println(set1)
    println("------------------------")
    //3. 删除元素
    val flag2 = set1.remove(10)
    println(flag2)
    println(set1)
    println("------------------------")
    //4. 合并两个set
    val set3 = mutable.Set(13, 12, 13, 31, 43, 37, 38)
    println(set1)
    println(set3)
    println("------------------------")
    val set4 = set1 ++ set3
    println(set1)
    println(set3)
    println(set4)
    println("------------------------")
    set3 ++= set1
    println(set1)
    println(set3)
  }
}
```

```
Set(33, 34, 13, 42, 23)
------------------------
Set(33, 34, 13, 42, 23)
Set(33, 34, 13, 42, 11, 23)
Set(33, 34, 13, 42, 11, 23)
true
Set(33, 34, 13, 42, 10, 11, 23)
------------------------
true
Set(33, 34, 13, 42, 11, 23)
------------------------
Set(33, 34, 13, 42, 11, 23)
Set(12, 37, 13, 31, 38, 43)
------------------------
Set(33, 34, 13, 42, 11, 23)
Set(12, 37, 13, 31, 38, 43)
Set(33, 12, 37, 34, 13, 31, 38, 42, 43, 11, 23)
------------------------
Set(33, 34, 13, 42, 11, 23)
Set(33, 12, 37, 34, 13, 31, 38, 42, 43, 11, 23)
```



## 5. Map集合

### 1. 不可变map

```scala
object Test08_ImmutableMap {
  def main(args: Array[String]): Unit = {
    //1. 创建map
    val map1: Map[String, Int] = Map("a" -> 13, "b" -> 25, "hello" -> 3)
    println(map1)
    println(map1.getClass)  //类的表达
    println("-----------------------------")
    //2. 遍历元素
    map1.foreach(println)
    map1.foreach((kv: (String, Int)) => println(kv))
    println("-----------------------------")
    //3. 访问元素: 取map中所有的key或value
    for (key <- map1.keys){
      println(s"$key ----> ${map1.get(key)}")
    }
    //4. 访问某一个key的value
    println("a: " + map1("a"))
    println("c: " + map1.get("c"))
    println("c: " + map1.getOrElse("c", 0)) //如果没有key对应的value,则返回指定的数字
  }
}
```

```
Map(a -> 13, b -> 25, hello -> 3)
class scala.collection.immutable.Map$Map3
-----------------------------
(a,13)
(b,25)
(hello,3)
(a,13)
(b,25)
(hello,3)
-----------------------------
a ----> Some(13)
b ----> Some(25)
hello ----> Some(3)
a: 13
c: None
c: 0
```

### 2. 可变map

```scala
import scala.collection.mutable

object Test09_MutableMap {
  def main(args: Array[String]): Unit = {
    //1. 创建可变map
    val map1: mutable.Map[String, Int] = mutable.Map("a" -> 13, "b" -> 23, "hello" -> 32)
    println(map1)
    println(map1.getClass)
    println("-----------------------")
    //2. 添加元素
    map1.put("c", 9)
    map1.put("d", 19)
    println(map1)

    map1 += (("e", 3))
    println(map1)
    println("-----------------------")
    //3. 删除元素
    println(map1("c"))
    map1.remove("c")
    println(map1.getOrElse("c", 0))

    map1 -= "d"
    println(map1)
    println("-----------------------")
    //4. 修改元素
    map1.update("c", 9)
    map1.update("e", 10)
    println(map1)
    println("-----------------------")
    //5. 合并集合
    val map2: mutable.Map[String, Int] = mutable.Map("aaa" -> 13, "b" -> 23, "hello" -> 1)
    map1 ++= map2 //将map2中的元素添加到map1中,如果map2中也存在map1中的key,map1中对应的value将会更新为map2中的value
    println(map1)
    println(map2)
    println("-----------------------")
    val map3 = map1 ++ map2
    println(map1)
    println(map2)
    println(map3)
  }
}
```

```
Map(b -> 23, a -> 13, hello -> 32)
class scala.collection.mutable.HashMap
-----------------------
Map(b -> 23, d -> 19, a -> 13, c -> 9, hello -> 32)
Map(e -> 3, b -> 23, d -> 19, a -> 13, c -> 9, hello -> 32)
-----------------------
9
0
Map(e -> 3, b -> 23, a -> 13, hello -> 32)
-----------------------
Map(e -> 10, b -> 23, a -> 13, c -> 9, hello -> 32)
-----------------------
Map(e -> 10, aaa -> 13, b -> 23, a -> 13, c -> 9, hello -> 1)
Map(b -> 23, aaa -> 13, hello -> 1)
-----------------------
Map(e -> 10, aaa -> 13, b -> 23, a -> 13, c -> 9, hello -> 1)
Map(b -> 23, aaa -> 13, hello -> 1)
Map(e -> 10, b -> 23, aaa -> 13, a -> 13, c -> 9, hello -> 1)
```

## 6. 元组

可以存放各种相同或不同类型的数据, 即将多个无关的数据封装为一个整体, 称为元组

<font color="red">注意: 元组中最大只能有22个元素</font>

```scala
object Test10_Tuple {
  def main(args: Array[String]): Unit = {
    //1. 创建元组
    val tuple: (String, Int, Char, Boolean) = ("hello", 9, 'a', true)
    println(tuple)
    //2. 访问数据
    println(tuple._1)
    println(tuple._2)
    println(tuple._3)
    println(tuple._4)

    println(tuple.productElement(1))
    println("-----------------------")
    //3. 遍历元组数据
    for ( elem <- tuple.productIterator){
      println(elem)
    }
    println("-----------------------")
    //4. 嵌套元组
    val mulTuple = (12, 0.2, "hello", (23, "scala"), 29)
    println(mulTuple._4._2)
  }

}
```

```
(hello,9,a,true)
hello
9
a
true
9
-----------------------
hello
9
a
true
-----------------------
scala
```

## 7. 集合常用函数

### 1. 基本属性和常用操作

```scala
object Test11_CommonOp {
  def main(args: Array[String]): Unit = {
    val list = List(1, 2, 3, 4, 5)
    val set = Set(2, 3, 43, 4, 42)
    //1. 获取集合长度
    println(list.length)
    println("-----------------------")
    //2. 获取集合大小
    println(set.size)
    println("-----------------------")
    //3. 循环遍历
    for (elem <- list) println(elem)

    set.foreach(println)
    println("-----------------------")
    //4. 迭代器
    for (elem <- list.iterator) println(elem)
    println("-----------------------")
    //5. 生成字符串
    println(list)
    println(set)
    println(list.mkString("--"))
    println("-----------------------")
    //6. 是否包含
    println(list.contains(2))
    println(set.contains(9))
  }
}
```

```
5
-----------------------
5
-----------------------
1
2
3
4
5
42
2
3
43
4
-----------------------
1
2
3
4
5
-----------------------
List(1, 2, 3, 4, 5)
Set(42, 2, 3, 43, 4)
1--2--3--4--5
-----------------------
true
false
```



### 2. 衍生集合

```scala
object Test12_DerivedCollection {
  def main(args: Array[String]): Unit = {
    val list1 = List(1, 2, 3, 4, 5, 6, 32, 23)
    val list2 = List(2, 3, 4, 5, 8, 9, 12, 34)
    //1. 获取集合的头
    println("list1 head: " + list1.head)
    //2. 获取集合的尾  --> 去掉头之后的所有元素为尾
    println("list1 tail: " + list1.tail)
    //3. 集合最后一个数据
    println("list2 last: " + list2.last)
    //4. 集合初始数据(不包含最后一个)
    println("list2初始数据: " + list2.init)
    //5. 反转
    println("list1 reverse: " + list1.reverse)
    //6. 取前(后)n个元素
    println("list1取前面3个元素: " + list1(3))
    println("list1从右边开始取4个元素: " + list1.takeRight(4))
    //7. 去掉前(后)n个元素
    println("list1去掉前3个元素: " + list1.drop(3))
    println("list1从右边开始去掉4个元素: " + list1.dropRight(4))
    //8. 并集
    val union = list1.union(list2)
    println("list1 and list2 union: " + union)
    println(list1 ::: list2)
    //如果是set做并集,会去重
    val set1 = Set(1, 2, 4, 5, 8, 9)
    val set2 = Set(1, 3, 5, 9, 0, 23)
    val union2 = set1.union(set2)
    println("set1 and set2 union: " + union2)
    //9. 交集
    val intersection = list1.intersect(list2)
    println("list1 and list2 intersection: " + intersection)
    //10. 差集
    val diff1 = list1.diff(list2)
    val diff2 = list2.diff(list1)
    println("存在list1,不存在于list2中的元素: " + diff1)
    println("存在list2,不存在于list1中的元素: " + diff2)
    //11. 拉链(对应位置的元素配对形成二元组,多出来的元素去掉)
    println("list1 zip list2: " + list1.zip(list2))
    println("list2 zip list1: " + list2.zip(list1))
    //12. 滑窗(指定长度的窗口滑动形成一个一个新的list)
    println("list1 sliding: ")
    for (elem <- list1.sliding(3)){
      println(elem)
    }
    //第一个参数是窗口大小,第二个参数为滑动的步长
    println("list2 sliding:")
    for (elem <- list2.sliding(4, 2))
      println(elem)

  }
}
```

```
list1 head: 1
list1 tail: List(2, 3, 4, 5, 6, 32, 23)
list2 last: 34
list2初始数据: List(2, 3, 4, 5, 8, 9, 12)
list1 reverse: List(23, 32, 6, 5, 4, 3, 2, 1)
list1取前面3个元素: 4
list1从右边开始取4个元素: List(5, 6, 32, 23)
list1去掉前3个元素: List(4, 5, 6, 32, 23)
list1从右边开始去掉4个元素: List(1, 2, 3, 4)
list1 and list2 union: List(1, 2, 3, 4, 5, 6, 32, 23, 2, 3, 4, 5, 8, 9, 12, 34)
List(1, 2, 3, 4, 5, 6, 32, 23, 2, 3, 4, 5, 8, 9, 12, 34)
set1 and set2 union: Set(0, 5, 1, 9, 2, 3, 23, 8, 4)
list1 and list2 intersection: List(2, 3, 4, 5)
存在list1,不存在于list2中的元素: List(1, 6, 32, 23)
存在list2,不存在于list1中的元素: List(8, 9, 12, 34)
list1 zip list2: List((1,2), (2,3), (3,4), (4,5), (5,8), (6,9), (32,12), (23,34))
list2 zip list1: List((2,1), (3,2), (4,3), (5,4), (8,5), (9,6), (12,32), (34,23))
list1 sliding: 
List(1, 2, 3)
List(2, 3, 4)
List(3, 4, 5)
List(4, 5, 6)
List(5, 6, 32)
List(6, 32, 23)
list2 sliding:
List(2, 3, 4, 5)
List(4, 5, 8, 9)
List(8, 9, 12, 34)
```



### 3. 集合计算简单函数

```scala
object Test13_SimpleFunction {
  def main(args: Array[String]): Unit = {
    val list1 = List(1, 2, 3, 4, 21, 8, 9)
    val list2 = List(("a", 5), ("b", 1), ("c", 9), ("d", 10), ("e", 92))
    //1. 求和
    var sum = 0
    for (elem <- list1) {
      sum += elem
    }
    println("method1 list1 sum: " + sum)

    println("method2 list1 sum: " + list1.sum)
    //2. 求乘积
    println("list1 multiply : " + list1.product)
    //3. 求最大值
    println("list1 max: " + list1.max)
    //加入list中的元素比较复杂,可以使用maxBy取最大值
    println("list2 max: " + list2.maxBy( (tuple: (String, Int)) => tuple._2)) //取list2中的元组的第二元素的最大值
    println("list2 max simple type: " + list2.maxBy( _._2))
    //4. 求最小值
    println("list1 min: " + list1.min)
    println("list2 min: " + list2.minBy(_._2))
    //5. 排序
    //1) sorted
    val sortedList = list1.sorted
    println("list1从小到大排序 method1: " + sortedList)
    //从大到小排序
    println("list1 从大到小排序 sorted method1: " + list1.sorted.reverse)
    //传入隐式参数
    println("list1从大到小排序 method2: " + list1.sorted(Ordering[Int].reverse))
    println("list2 sorted num1: " + list2.sorted)  //默认以元组的第一个元素的大小从小到大排序
    //2)sortBy
    println("list2 sorted num2: " + list2.sortBy(_._2))  //以元组的第二个元素从小到大排序
    println("list2 sorted num2: " + list2.sortBy(_._2)(Ordering[Int].reverse)) //以元组的第二个元素从大到小排序
    //3) sortWith
    println("list1 从小到大排序 method2: " + list1.sortWith((a: Int, b: Int) => {a < b}))
    println("list1 sorted simple type: " + list1.sortWith( _ < _ ))
    println("list sorted simple type: " + list1.sortWith( _ > _ ))
  }
}
```

```
method1 list1 sum: 48
method2 list1 sum: 48
list1 multiply : 36288
list1 max: 21
list2 max: (e,92)
list2 max simple type: (e,92)
list1 min: 1
list2 min: (b,1)
list1从小到大排序 method1: List(1, 2, 3, 4, 8, 9, 21)
list1 从大到小排序 sorted method1: List(21, 9, 8, 4, 3, 2, 1)
list1从大到小排序 method2: List(21, 9, 8, 4, 3, 2, 1)
list2 sorted num1: List((a,5), (b,1), (c,9), (d,10), (e,92))
list2 sorted num2: List((b,1), (a,5), (c,9), (d,10), (e,92))
list2 sorted num2: List((e,92), (d,10), (c,9), (a,5), (b,1))
list1 从小到大排序 method2: List(1, 2, 3, 4, 8, 9, 21)
list1 sorted simple type: List(1, 2, 3, 4, 8, 9, 21)
list sorted simple type: List(21, 9, 8, 4, 3, 2, 1)
```



### 4. 集合计算高级函数

1. 过滤

   遍历一个集合并从中获取满足指定条件的元素组成一个新的集合

2. 转化/映射

   将集合中的每一个元素映射到某一个函数

3. 扁平化

   拆成最小的元素,再合成一个集合

4. 扁平化+映射: 先进行map操作, 再进行扁平化

   集合中的每个元素的子元素映射到某个函数并返回新集合

5. 分组(group)

   按照指定的规则对集合的元素进行分组

   

```scala
object Test14_HighLevelFunction_Map {
  def main(args: Array[String]): Unit = {
    val list = List(2, 3, 5, 6, 7, 9, 10)
    //1. 过滤
    //选取偶数
    val evenList = list.filter((elem: Int) => {elem % 2 ==0})
    println("list 选取偶数: " + evenList)
    println("list 选取奇数: " + list.filter(_ % 2 == 1))
    //2. map
    //把集合中的每个数乘2
    println("list * 2: " + list.map(_ * 2))
    println("list * list: " + list.map(x => x * x))  //集合每个数平方
    //3. 扁平化
    val nestedList: List[List[Int]] = List(List(1, 3, 4), List(2, 5, 6), List(6, 7, 2))
    val flatList = nestedList(0) ::: nestedList(1) ::: nestedList(2)
    println("list扁平化 method1: " + flatList)

    val flatList2 = nestedList.flatten
    println("list扁平化 method2: " + flatList2)
    //4. 扁平映射
    //将一组字符串进行分词, 并保存成单词的列表
    val strings: List[String] = List("hello world", "hello scala", "hello java")
    val splitList: List[Array[String]] = strings.map(string => string.split(" "))  //分词
    val flattenList = splitList.flatten  //打散扁平
    println("扁平映射 method1: " + flattenList)

    //扁平映射
    val flatmapList = strings.flatMap(_.split(" "))
    println("扁平映射 method2: " + flatmapList)

    //5. 分组groupBy
    //分成奇偶两组
    val groupMap = list.groupBy(_ % 2)
    val groupMap2: Map[String, List[Int]] = list.groupBy( data => {
      if (data % 2 ==0) "偶数" else "奇数"
    })
    println("分组1: " + groupMap)
    println("分组2: " + groupMap2)

    //给定一组词汇,按照单词的首字母进行分组
    val wordList = List("china", "america", "alice", "canada", "cary", "divide")
    println("按照单词的首字母分组: " + wordList.groupBy(_.charAt(0)))
  }
}
```

```
list 选取偶数: List(2, 6, 10)
list 选取奇数: List(3, 5, 7, 9)
list * 2: List(4, 6, 10, 12, 14, 18, 20)
list * list: List(4, 9, 25, 36, 49, 81, 100)
list扁平化 method1: List(1, 3, 4, 2, 5, 6, 6, 7, 2)
list扁平化 method2: List(1, 3, 4, 2, 5, 6, 6, 7, 2)
扁平映射 method1: List(hello, world, hello, scala, hello, java)
扁平映射 method2: List(hello, world, hello, scala, hello, java)
分组1: Map(0 -> List(2, 6, 10), 1 -> List(3, 5, 7, 9))
分组2: Map(偶数 -> List(2, 6, 10), 奇数 -> List(3, 5, 7, 9))
按照单词的首字母分组: Map(c -> List(china, canada, cary), a -> List(america, alice), d -> List(divide))
```

1. 简化(规约) : 得到最简单的操作
2. 折叠

```scala
object Test15_HiveLevelFunction_Reduce {
  def main(args: Array[String]): Unit = {
    val list = List(1, 3, 5, 6)
    //1. reduce(没有初值)
    println(list.reduce(_ + _))
    println(list.reduceLeft(_ + _)) //从坐往右加
    println(list.reduceRight(_ + _))  //从右往左加

    val list2 = List(3, 5, 5, 2, 9)
    println(list2.reduce(_ - _))
    println(list2.reduceLeft(_ - _))
    println(list2.reduceRight(_ - _))  //3 - (5 - (5 - (2 - 9)))
    println("-------------------------")
    //2. fold(需要单独传入一个初值)
    println(list.fold(10)(_ + _))  //10 + 1 + 3 + 5 +6
    println(list.foldLeft(10)(_ - _)) //10 - 1 - 3 - 5 - 6
    println(list.foldRight(11)(_ - _))  //1 - (3 - (5 - (6 - 11)))
  }
}
```

```
15
15
15
-18
-18
10
-------------------------
25
-5
8
```

### 5. 案例Merge

```scala
import scala.collection.mutable

object Test16_MergeMap {
  def main(args: Array[String]): Unit = {
    val map1 = Map("a" -> 1, "b" -> 2, "c" ->4)
    val map2 = mutable.Map("a" -> 1, "b" -> 4, "c" -> 8, "d" -> 9)

    val map3 = map1.foldLeft(map2)(
      (mergedMap, kv) => {
        val key = kv._1
        val value = kv._2
        mergedMap(key) = mergedMap.getOrElse(key, 0) + value
        mergedMap
      }
    )
    println(map3)
  }
}
```

```
Map(b -> 6, d -> 9, a -> 2, c -> 12)
```



### 6. 普通WordCount案例

```scala
object Test17_CommonWordCount {
  def main(args: Array[String]): Unit = {
    val stringList: List[String] = List(
      "hello",
      "hello world",
      "hello scala",
      "hello spark from scala",
      "hello flink from scala"
    )

    //1. 对字符串进行切分, 得到一个打散所有单词的列表
    val wordList: List[String] = stringList.flatMap(_.split(" "))
    println(wordList)
    //2. 相同的单词进行分组
    val groupMap: Map[String, List[String]] = wordList.groupBy(word => word)
    println(groupMap)
    //3. 对分组之后的list取长度,得到每个单词的个数
    val countMap: Map[String, Int] = groupMap.map(kv => (kv._1, kv._2.length))
    println(countMap)
    //4. 将map转换为List,并排序取前三
    val sortList: List[(String, Int)] = countMap.toList.sortWith(_._2 > _._2).take(3)
    println(sortList)
  }

}
```

```
List(hello, hello, world, hello, scala, hello, spark, from, scala, hello, flink, from, scala)
Map(world -> List(world), flink -> List(flink), spark -> List(spark), scala -> List(scala, scala, scala), from -> List(from, from), hello -> List(hello, hello, hello, hello, hello))
Map(world -> 1, flink -> 1, spark -> 1, scala -> 3, from -> 2, hello -> 5)
List((hello,5), (scala,3), (from,2))
```

### 7. 复杂WordCount案例

```scala
object Test18_ComplexWordCount {
  def main(args: Array[String]): Unit = {
    val tupleList: List[(String, Int)] = List(
      ("hello", 1),
      ("hello world", 2),
      ("hello scala", 2),
      ("hello spark from scala", 4),
      ("hello flink from scala", 3)
    )

    //思路一: 直接展开为普通版本
    val newStringList: List[String] = tupleList.map(
      kv => {
        (kv._1 + " ") * kv._2
      }
    )
    println(newStringList)
    //接下来操作与普通版本完全一致
    val wordCountList: List[(String, Int)] = newStringList.flatMap(_.split(" "))  //空格分词
      .groupBy(word => word)  //按照单词分组
      .map( kv => (kv._1, kv._2.size))  //统计出每个单词的个数
      .toList
      .sortBy(_._2)(Ordering[Int].reverse)
      .take(3)
    println(wordCountList)
    println("-----------------------------")
    //思路二: 直接基于欲统计的结果进行转换
    //1. 将字符串打散为单词, 并结合对应的个数包装成二元组
    val preCountList: List[(String, Int)] = tupleList.flatMap(
      tuple => {
        val strings: Array[String] = tuple._1.split(" ")
        strings.map( word => (word, tuple._2))
      }
    )
    println(preCountList)
    //2. 对二元组按照单词进行分组
    val preCountMap: Map[String, List[(String, Int)]] = preCountList.groupBy( _._1 )
    println(preCountMap)
    //3. 叠加每个单词欲统计的个数值
    val countMap: Map[String, Int] = preCountMap.mapValues(
      tupleList => tupleList.map(_._2).sum
    )
    println(countMap)
    //4. 转换成list, 排序取前三
    val countList = countMap.toList.sortWith(_._2 > _._2).take(3)
    println(countList)
  }
}
```

```
List(hello , hello world hello world , hello scala hello scala , hello spark from scala hello spark from scala hello spark from scala hello spark from scala , hello flink from scala hello flink from scala hello flink from scala )
List((hello,12), (scala,9), (from,7))
-----------------------------
List((hello,1), (hello,2), (world,2), (hello,2), (scala,2), (hello,4), (spark,4), (from,4), (scala,4), (hello,3), (flink,3), (from,3), (scala,3))
Map(world -> List((world,2)), flink -> List((flink,3)), spark -> List((spark,4)), scala -> List((scala,2), (scala,4), (scala,3)), from -> List((from,4), (from,3)), hello -> List((hello,1), (hello,2), (hello,2), (hello,4), (hello,3)))
Map(world -> 2, flink -> 3, spark -> 4, scala -> 9, from -> 7, hello -> 12)
List((hello,12), (scala,9), (from,7))
```

## 8. 队列

```scala
import scala.collection.immutable.Queue
import scala.collection.mutable

object Test19_Queue {
  def main(args: Array[String]): Unit = {
    //1. 创建一个可变队列
    val queue = new mutable.Queue[String]()
    //入队
    queue.enqueue("a", "b", "c", "d")
    println(queue)
    //出队
    println(queue.dequeue())
    println(queue)
    println("=========================")

    //不可变队列
    val queue2 = Queue("a", "b", "c")
    val queue3 = queue2.enqueue("d")
    println(queue2)
    println(queue3)
  }
}
```

```
Queue(a, b, c, d)
a
Queue(b, c, d)
=========================
Queue(a, b, c)
Queue(a, b, c, d)
```

## 9. 并行集合

Scala为了充分使用多核CPU, 提供了并行集合(有别于前面的串行集合), 用于多核环境的并行计算

```scala
import scala.collection.immutable
import scala.collection.parallel.immutable.ParSeq

object Test20_Parallel {
  def main(args: Array[String]): Unit = {
    //穿行集合
    val result: immutable.IndexedSeq[Long] = (1 to 100).map(
      x => Thread.currentThread.getId
    )
    println(result)
    //并行集合
    val result2: ParSeq[Long] = (1 to 100).par.map(
      x => Thread.currentThread.getId
    )
    println(result2)
  }
}
```

```
Vector(1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1)
ParVector(11, 11, 11, 11, 11, 11, 11, 11, 11, 11, 11, 11, 11, 11, 11, 11, 11, 11, 11, 11, 11, 11, 11, 11, 11, 13, 13, 13, 13, 13, 13, 13, 13, 13, 13, 13, 13, 13, 13, 13, 13, 13, 13, 13, 13, 13, 13, 13, 13, 13, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 14, 14, 14, 14, 14, 14, 14, 14, 14, 14, 14, 14, 14, 14, 14, 14, 14, 14, 14, 14, 14, 14, 14, 14, 14)
```



# 模式匹配

类似于Java中的switch语法

## 1. 基本语法

使用match case

```scala
object Test01_PatternMatchBase {
  def main(args: Array[String]): Unit = {
    //1. 基本定义语法
    val x: Int = 2
    val y: String = x match {
      case 1 => "one"
      case 2 => "two"
      case 3 => "three"
      case _ => "other"
    }
    println(y)
    println("---------------------------")
    //2. 示例: 用模式匹配实现简单的二元运算
    val a = 24
    val b = 29
    def matchDualOP(op: Char): Int = op match {
      case '+' => a + b
      case '-' => a - b
      case '*' => a * b
      case '/' => a / b
      case '%' => a % b
      case _ => -1
    }
    println(matchDualOP('+'))
    println(matchDualOP('-'))
    println(matchDualOP('*'))
    println(matchDualOP('/'))
    println(matchDualOP('%'))
    println(matchDualOP(','))
  }
}
```

```
two
---------------------------
53
-5
696
0
24
-1
```

1. 如果所有的case都不匹配, 则会执行case _分支
2. 每个case中不需要使用break语句, 会自动中断
3. match case可以匹配任何类型



## 2. 模式守卫

如果想要表达匹配的某个范围的数据, 就需要在模式匹配中增加条件守卫

```scala
//3. 模式守卫
    def abs(num: Int): Int = {
      num match {
        case i if i >= 0 => i
        case i if i < 0 => -i
      }
    }
    println(abs(21))
    println(abs(-23))
    println(abs(0))
```



## 3. 模式匹配类型

模式匹配可以匹配所有的字面量, 包括字符串, 字符, 数组, 布尔类型等

```scala
object Test02_MatchTypes {
  def main(args: Array[String]): Unit = {
    //1. 匹配常量
    def describeConst(x: Any): String = x match {
      case 1 => "Num one"
      case "hello" => "String hello"
      case true => "Boolean true"
      case '+' => "Char +"
      case _ => ""
    }
    println(describeConst("hello"))
    println(describeConst('+'))
    println(describeConst(0.1))

    println("=========================")
    //2. 匹配类型
    def describeType(x: Any): String = x match {
      case i: Int => "Int " + i
      case s: String => "String " + s
      case list: List[String] => "List "+ list
      case array: Array[Int] => "Array[Int] " + array.mkString("-")
      case a => "Something else: " + a
    }
    println(describeType(35))
    println(describeType("hello"))
    println(describeType(List("hello", "hi")))
    println(describeType(List(12, 34)))
    println(describeType(Array("hello", "hi")))
    println(describeType(Array(1, 2)))
    println("=========================")
    //3. 匹配数组类型
    for (arr <- List(
        Array(0),
        Array(1, 3),
        Array(1, 0, 0),
        Array(1, 1, 0),
        Array(2, 3, 5, 91),
        Array("hello", 20, 30)
    )){
      val result = arr match {
        case Array(0) => "0"
        case Array(1, 3) => "Array(1, 3)"
        case Array(x, y) => "Array: " + x + ", " + y
        case Array(0, _*) => "以0开头的数组"
        case Array(_, 1, _) => "中间为1的三元数组"
        case _ => "something else"
      }
      println(result)
    }
    println("=========================")

    //4. 匹配列表
    //方式一
    for(list <- List(
        List(0),
        List(1, 0),
        List(0, 0, 0),
        List(1, 1, 0),
        List(22)
    )){
      val result = list match{
        case List(0) => "0"
        case List(x, y) => "List(x, y): " + x + ", " + y
        case List(0, _*) => "List(0, ...)"
        case List(a) => "List(a): " + a
        case _ => "something else"
      }
      println(result)
    }
    println("=========================")
    //方式二
    val list = List(1, 2, 3, 6, 34)
    val list2 = List(2)
    list match {
      case first :: second :: rest => println(s"first: $first, second: $second, rest: $rest")
      case _ => println("something else")
    }
    list2 match {
      case first :: second :: rest => println(s"first: $first, second: $second, rest: $rest")
      case _ => println("something else")
    }
    println("=========================")
    //5. 匹配元组
    for ( tuple <- List(
      (0, 1),
      (0, 0),
      (1, 0, 1),
      (0, 1, 1),
      (1, 32, 42),
      ("hello", true, 0.1)
    )){
      val result = tuple match{
        case (a, b) => "" + a + ", " + b
        case (0, _) => "(0, _"
        case (a, 1, _) => "(a, 1, _) " + a
        case (x, y, z) => "(x, y, z): " + x + " " + y + " " + z

        case _ => "something else"
      }
      println(result)
    }
  }
}
```

```
String hello
Char +

=========================
Int 35
String hello
List List(hello, hi)
List List(12, 34)
Something else: [Ljava.lang.String;@1134affc
Array[Int] 1-2
=========================
0
Array(1, 3)
something else
中间为1的三元数组
something else
something else
=========================
0
List(x, y): 1, 0
List(0, ...)
something else
List(a): 22
=========================
first: 1, second: 2, rest: List(3, 6, 34)
something else
=========================
0, 1
0, 0
(x, y, z): 1 0 1
(a, 1, _) 0
(x, y, z): 1 32 42
(x, y, z): hello true 0.1
```

关于匹配元组的额外的用法:

```scala
object Test03_MatchTupleExtend {
  def main(args: Array[String]): Unit = {
    //1. 在变量声明是匹配
    val (x, y) = (10, "hello")
    println(s"x: $x, y: $y")

    val List(first, second, _*) = List(23, 42, 6, 23)
    println(s"first: $first, second: $second")

    val fir :: sec :: rest = List(23, 43, 5, 39)
    println(s"first: $fir, second: $sec, rest: $rest")
    println("=========================")
    //2. for推到式中进行模式匹配
    val list: List[(String, Int)] = List(("a", 12), ("b", 34), ("c", 82), ("a", 23))
    //1) 原本的遍历模式
    for (elem <- list){
      println(elem._1 + " " + elem._2)
    }
    println("=========================")
    //2) 将list元素直接定义为元组,对变量赋值
    for ((word, count) <- list){
      println(word + " " + count)
    }
    println("=========================")
    //3) 可以不考虑某个位置的变量, 只遍历key或者value
    for ((word, _) <- list){
      println(word)
    }
    println("=========================")
    //4) 可以指定某个位置的值必须是多少
    for (("a", count) <- list) {
      println(count)
    }
  }
}
```

```
x: 10, y: hello
first: 23, second: 42
first: 23, second: 43, rest: List(5, 39)
=========================
a 12
b 34
c 82
a 23
=========================
a 12
b 34
c 82
a 23
=========================
a
b
c
a
=========================
12
23
```

**匹配对象及样例类**

```scala
object Test04_MatchObject {
  def main(args: Array[String]): Unit = {
    val student = new Student("alice", 19)

    //针对对象实例的内容进行匹配
    val result = student match{
      case Student("alice", 19) => "Alice, 19"
      case _ => "else"
    }
    println(result)
  }
}

//定义类
class Student(val name: String, val age: Int)
//定义伴生对象
object Student {
  def apply(name: String, age: Int): Student = new Student(name, age)
  //必须实现一个unapply方法, 用来对对象属性进行拆解
  def unapply(student: Student): Option[(String, Int)] = {
    if (student == null)  None else Some(student.name, student.age)
  }
}
```

直接定义样例类自动实现伴生对象的作用:

```scala
object Test05_MatchCaseClass {
  def main(args: Array[String]): Unit = {
    val student = Student1("alice", 19)

    //针对对象实例的内容进行匹配
    val result = student match{
      case Student1("alice", 19) => "Alice, 19"
      case _ => "else"
    }
    println(result)
  }
}

//定义样例类
case class Student1(name: String, age: Int)
```

> Alice, 19



## 6. 偏函数中的模式匹配(了解)

尚硅谷大数据技术之Scala入门到精通教程136集



# 异常

1. try-catch-finally 与Java 用法相似
2. throw关键字, 抛出一个异常对象, 所有异常都是Throwable的子类型. throw表达式是有类型的, 就是Nothing, 因为Nothing是所有类型的子类型, 所以throw表达式可以用在需要类型的地方
3. thorws关键字声明异常

```scala
object Test01_Exception {
  def main(args: Array[String]): Unit = {
    try{
      val n = 10 / 0
    }catch {
      case e: ArithmeticException =>
        println("发生算数异常")
      case e: Exception =>
        println(" 发生一般异常")
    }finally {
      println("处理结束")
    }
  }
}
```

> 发生算数异常
> 处理结束



# 隐式转换

**<font color="red">当编译器第一次编译失败时, 会在当前的环境中查找能让代码编译通过的方法, 用于将类型进行转换, 实现二次编译</font>**

1.  隐式函数

   可以在不需要改任何代码的情况下, 扩展某个类的功能

2. 隐式参数

   普通方法或函数中的参数通过```implicit```关键字声明

3. 隐式类

   使用```implicit```修饰的类

4. 隐式解析机制

   (1) 首先会在当前代码作用域下查找隐式实体(隐式方法, 隐式类, 隐式对象). (一般是这种情况)

   (2) 如果第一条规则查找隐式实体失败, 会继续在隐式参数的类型的作用域里查找. 类型的作用域是指与**该类型相关联的全部伴生对象**以及**该类型所在包的包对象**

```scala
object Test02_Implicit {
  def main(args: Array[String]): Unit = {
    val new12 = new MyRichInt(12)
    println(new12.myMax(15))

    //1. 隐式函数
    implicit def convert(num: Int): MyRichInt = new MyRichInt(num)
    println(12.myMax(15))
    println("======================")
    //2. 隐式类
    implicit class MyRichInt2(val self: Int){
      //自定义比较大小的方法
      def myMax2(n: Int): Int = if (n < self) self else n
      def myMin2(n: Int): Int = if (n < self) n else self
    }
    println(12.myMin2(15))
    println("======================")
    //3. 隐式参数(在同一作用域范围内, 同类型隐式参数只能定义一个,隐式值会覆盖默认值
    implicit val str: String = "alice"
    implicit val age: Int = 19
    def sayHello(name: String): Unit = {
      println("hello, " + name)
    }
    def sayHi(implicit name: String = "atguigu"): Unit = {
      println("hi, " + name)
    }
    sayHello("alice")
    sayHi

    //简便写法
    def hiAge(): Unit = {
      println("hi, " + implicitly[Int])
    }
    hiAge()
  }
}

//自定义类
class MyRichInt(val self: Int){
  //自定义比较大小的方法
  def myMax(n: Int): Int = if (n < self) self else n
  def myMin(n: Int): Int = if (n < self) n else self
}
```

```
15
15
======================
12
======================
hello, alice
hi, alice
hi, 19
```



# 泛型

**1. 协变和逆变**

```scala
class MyList[+T]{}  //协变
class MyList[-T]{}  //逆变
class MyList[T]{}  //不变
```

1. 协变: son是father的子类, 则MyList[Son] 也作为MyList[Father]的"子类"
2. 逆变: son是father的子类, 则MyList[Son] 也作为MyList[Father]的"父类"
3. 不变: son是father的子类, 则MyList[Son] 与MyList[Father]"无父子关系"



**2. 泛行上下限**

```scala
class PersonList[T<:Preson]{}  //泛行上限
class PersonList[T>:Person]{}  //泛行下限
```

泛型的上下限的作用是对传入的泛型进行限定

**3. 上下文限定**

```scala
def[A:B](a: A) = println(a) 
//等同于
def[A](a: A)(implicit arg:B[A]) = println(a)
```

上下文限定是将泛型和隐式转换的结合产物, 以下两者功能相同, 使用上下文限定[A: Ordering]之后, 方法内无法使用隐式参数名调用隐式参数, 需要通过```implicity[Ordering[A]]```获取隐式变量, 如果此时无法查找到对应类型的隐式变量, 会发生错误
