# Spark概述



spark是一种基于内存的快速、通用、可扩展的大数据分析计算引擎。

Spark是计算框架

一次性数据计算：

框架在处理数据的时候，会从存储设备中读取数据，及逆行逻辑操作，然后将处理的结果重新存储到介质中

<font color="red">spark将计算结果放在了内存中</font>

**Spark和Hadoop的根本差异是多个作业之间的数据通信问题： Spark多个作业之间数据通信是基于内存，而Hadoop是基于磁盘**



## Hadoop与spark的区别

**1. 原理比较**

Hadoop和Spark都是并行计算框架，两者都可以使用MR模型进行计算（spark有内置的独立的计算引擎）

**2. 数据的存储与处理**

Hadoop：是一个分布式系统的基础架构，它可以独立完成数据的存储（HDFS）和处理（MR）工作

spark：是一个专门用来对那些分布式存储的大数据进行处理的工具，他没有提供文件管理系统，自身也不会对数据进行存储。它需要借助其他分布式文件存储系统才能够动作，最经典的就是Hadoop的HDFS

**3. 处理速度**

Hadoop：在磁盘级计算，在计算时需要读取磁盘中的数据，它只能用来处理离线（静态）数据

spark：在内存中以接近“实时”的时间完成所有的数据分析，Spark的批处理速度比MR近快10倍，内存中的数据分析速度快近100倍

**4. 恢复性**

Hadoop：将每次处理后的数据写入到磁盘中，对应系统错误具有天生优势

Spark：数据对象存储在分布式数据集（RDD）当中。RDD也提供了完整的灾难恢复功能

**5. 处理数据**
Hadoop：一般用来处理离线（静态）数据，对于流式（实时）数据处理能力较差

Spark：通常适合处理流式数据

**6. 中间结果**

Hadoop：将中间结果保存到HDFS中，每次MR都需要进行读取-调用

Spark：将中间结果保存到内存中，如果内存不够再放入到磁盘中，不放入HDFS，避免大量的IO和读写操作



## 内置模块

- Spark Core（后结模块的基础）
- Spark SQL（分析结构化数据）
- Spark Streaming（实时分析处理）
- Spark MLlib（机器学习）
- Spark GraphX（图学习）

## 特点

1. 批/流式数据处理
1. 使用SQL进行分析
1. 对大规模数据进行科学计算
1. 机器学习



# Spark快速上手

## 1. 创建Maven项目

增加Scala插件

（1）创建Maven项目

![image-20220905100109278](Spark image\image-20220905100109278.png)

![image-20220905100340947](Spark image\image-20220905100340947.png)

创建模块

![image-20220905100721448](Spark image\image-20220905100721448.png)

![image-20220905100842685](Spark image\image-20220905100842685.png)

![image-20220905100906216](Spark image\image-20220905100906216.png)

（2）增加scala

安装插件

![image-20220905101101958](Spark image\image-20220905101101958.png)

从官网中下载，官网地址 http://www.scala-lang.org/downloads 下载 Scala 二进制包(页面底部)

![image-20220905101214799](Spark image\image-20220905101214799.png)

![image-20220905101249420](Spark image\image-20220905101249420.png)

下载之后解压,然后配置环境变量

![image-20220905101358401](Spark image\image-20220905101358401.png)

回到idea，File-->Project Structure-->Global Libraries , 选择刚刚下载的scala解压路径,并应用到spark-core上

![image-20220905101618352](Spark image\image-20220905101618352.png)

![image-20220905101739917](Spark image\image-20220905101739917.png)

（3）开发scala

创建一个包,再创建一个Scala Class

![image-20220905102354728](Spark image\image-20220905102354728.png)

![image-20220905102746223](Spark image\image-20220905102746223.png)选择object

创建完成后,写入代码并运行,在控制台能成功打印出结果即说明环境搭建完成



## 2. WordCount

   (1) 环境搭建

   导入依赖

   ```
   <dependencies>
          <dependency>
              <groupId>org.apache.spark</groupId>
              <artifactId>spark-core_2.12</artifactId>
              <version>3.0.0</version>
          </dependency>
   </dependencies>
   ```

   编写代码

   ```java
   import org.apache.spark.{SparkConf, SparkContext}
   
   object Spark01_WordCount {
     def main(args: Array[String]): Unit = {
       //application
       //spark框架
       //TODO 建立和spark框架的连接
       //JDBC : Connection (setMaster表述的是spark框架运行的环境,local环境,即本地环境,setappname给应用起名
       val sparConf = new SparkConf().setMaster("local").setAppName("WordCount")
       val sc = new SparkContext(sparConf)
   
       //TODO 执行业务操作
       //TODO 关闭连接
       sc.stop()
     }
   }
   ```

   运行代码,在控制台打印出日志信息

   ![image-20220905112334502](Spark image\image-20220905112334502.png)

   (2) 编写代码

   准备文件

   ![image-20220905142534784](Spark image\image-20220905142534784.png)

   执行业务操作部分的代码

   ```java
   	//TODO 执行业务操作
       //1. 读取文件,获取一行一行的数据  hello world
       val lines : RDD[String] = sc.textFile("datas")
       //2. 将一行数据进行拆分,形成一个一个的单词(分词)  "hello world" ==> hello, world, hello world
       val words : RDD[String] = lines.flatMap(_.split(" "))
       //3. 将数据根据单词进行分组,便于统计  (hello, hello, hello), (world, world)
       val wordGroup : RDD[(String, Iterable[String])] = words.groupBy(word=>word)
       //4. 对分组后的数据进行转换  (hello, hello, hello), (world, world) ==> (hello, 3),(world, 2)
       val wordToCount = wordGroup.map{
         case (word, list) =>
           (word, list.size)
       }
       //5. 将转换结果采集到控制台打印
       val array: Array[(String, Int)] = wordToCount.collect()
       array.foreach(println)
   ```

   运行代码

   ![image-20220905142623197](Spark image\image-20220905142623197.png)

   

**不同的实现**

```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{SparkConf, SparkContext}

object Spark02_WordCount2 {
  def main(args: Array[String]): Unit = {
    //application
    //spark框架
    //TODO 建立和spark框架的连接
    //JDBC : Connection (setMaster表述的是spark框架运行的环境,local环境,即本地环境,setappname给应用起名
    val sparConf = new SparkConf().setMaster("local").setAppName("WordCount")
    val sc = new SparkContext(sparConf)

    //TODO 执行业务操作
    val lines : RDD[String] = sc.textFile("datas")
    val words : RDD[String] = lines.flatMap(_.split(" "))
    val wordToOne = words.map(
      word => (word, 1)
    )
    val wordGroup : RDD[(String, Iterable[(String, Int)])] = wordToOne.groupBy(t => t._1)
    val wordToCount = wordGroup.map{
      case (_, list) =>
        list.reduce(
          (t1, t2) => {(t1._1, t1._2 + t2._2)}
        )
    }
    val array: Array[(String, Int)] = wordToCount.collect()
    array.foreach(println)
    //TODO 关闭连接
    sc.stop()
  }
}
```

![image-20220913170634614](Spark%20image/image-20220913170634614-166306001374417.png)

使用spark实现:

```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{SparkConf, SparkContext}

object Spark03_WordCount2 {
  def main(args: Array[String]): Unit = {
    //TODO 建立和spark框架的连接
    val sparConf = new SparkConf().setMaster("local").setAppName("WordCount")
    val sc = new SparkContext(sparConf)

    //TODO 执行业务操作
    val lines : RDD[String] = sc.textFile("datas")
    val words : RDD[String] = lines.flatMap(_.split(" "))
    val wordToOne = words.map(
      word => (word, 1)
    )
    //Spark框架提供了更多的功能, 可以将分组和聚合使用一个方法实现
    //reduceByKey: 相同的key的数据, 可以对value进行reduce聚合
    val wordToCount = wordToOne.reduceByKey(_ + _)
    val array: Array[(String, Int)] = wordToCount.collect()
    array.foreach(println)
    //TODO 关闭连接
    sc.stop()
  }
}
```

在执行的过程中, 会产生大量的执行日志, 为了更好的查看程序的执行结果, 在项目的resources目录中创建log4j.properties文件, 添加日志配置信息: 

```
log4j.rootCategory=ERROR, console
log4j.appender.console=org.apache.log4j.ConsoleAppender
log4j.appender.console.target=System.err
log4j.appender.console.layout=org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern=%d{yy/MM/dd HH:mm:ss} %p %c{1}: %m%n

# Set the default spark-shell log level to ERROR. When running the spark-shell, the
# log level for this class is used to overwrite the root logger's log level, so that
# the user can have different defaults for the shell and regular Spark apps.
log4j.logger.org.apache.spark.repl.Main=ERROR

# Settings to quiet third party logs that are too verbose
log4j.logger.org.spark_project.jetty=ERROR
log4j.logger.org.spark_project.jetty.util.component.AbstractLifeCycle=ERROR
log4j.logger.org.apache.spark.repl.SparkIMain$exprTyper=ERROR
log4j.logger.org.apache.spark.repl.SparkILoop$SparkILoopInterpreter=ERROR
log4j.logger.org.apache.parquet=ERROR
log4j.logger.parquet=ERROR

# SPARK-9183: Settings to avoid annoying messages when looking up nonexistent UDFs in SparkSQL with Hive support
log4j.logger.org.apache.hadoop.hive.metastore.RetryingHMSHandler=FATAL
log4j.logger.org.apache.hadoop.hive.ql.exec.FunctionRegistry=ERROR
```



# spark运行环境

## 1. Local模式

不需要其他任何节点资源就可以在本地执行Spark代码的环境

将相应的spark-Hadoop的压缩包上传到Linux环境中，解压缩并启动



## 2. Standalone模式

实际工作中要将应用提交到对应的集群中去执行

只使用Spark自身节点运行的集群模式



## 3. Yarn模式



## 4. K8S & Mesos模式

Mesosphere是apache下的开源分布式资源管理框架，被称为分布式系统的内核



## 5. Windows模式

**1. 解压缩文件**

下载对应Hadoop版本的spark,[下载](https://archive.apache.org/dist/spark/spark-3.0.0/),然后解压缩

将文件`spark-3.0.0-bin-hadoop2.7.tgz`解压缩

![image-20221018170634863](Spark%20image/image-20221018170634863.png)

**2. 启动本地环境**

1. 执行解压缩文件路径下`bin`目录中的`spark-shell.cmd`文件,启动Spark本地环境

2. 在`bin`目录中创建`input`目录,并添加`word.txt`文件,在命令行中输入脚本代码

   ````scala
   sc.textFile("input/word.txt").flatMap(_.split(" ")).map((_, 1)).reduceByKey(_+_).collect
   ````

**3. 命令行提交应用**

在`E:\ProgramFiles\spark-3.0.0-bin-hadoop2.7\bin`目录下打开cmd,执行指令

```scala
spark-submit --class org.apache.spark.examples.SparkPi --master local[2] ../examples/jars/spark-examples_2.12-3.0.0.jar 10
```



# Spark运行架构

## 核心组件

**1. Driver**

<font color="red">计算相关的组件</font>

Spark驱动器节点,用于执行Spark任务中的main方法,负责实际代码的执行工作

主要负责:

- 将用户程序转化为作业(job)
- 在Executor之间调度任务
- 跟踪Executor的执行情况
- 通过UI展示查询运行情况

**2. Executor**

<font color="red">计算相关的组件</font>

集群中工作节点(Worker)的一个JVM进程,负责Spark作业中运行具体任务(Task),任务彼此之间相互独立

核心功能:

- 负责运行组成Spark应用的任务,并将结果返回给驱动器进程
- 通过自身的块管理器(Bolck Manager)为用户程序中要求缓存的RDD提供内存式存储

**3. Master & Worker**

<font color="red">资源相关的组件</font>

Spark 集群的独立部署环境中，不需要依赖其他的资源调度框架，自身就实现了资源调度的功能，所以环境中还有其他两个核心组件：Master 和 Worker，这里的 Master 是一个进程，主要负责资源的调度和分配，并进行集群的监控等职责，类似于 Yarn 环境中的 RM, 而Worker 呢，也是进程，一个 Worker 运行在集群中的一台服务器上，由 Master 分配资源对数据进行并行的处理和计算，类似于 Yarn 环境中 NM。

**4. ApplicationMaster**

Hadoop 用户向 YARN 集群提交应用程序时,提交程序中应该包含 ApplicationMaster，用于向资源调度器申请执行任务的资源容器 Container，运行用户自己的程序任务 job，监控整个任务的执行，跟踪整个任务的状态，处理任务失败等异常情况。

说的简单点就是，ResourceManager（资源）和 Driver（计算）之间的解耦合靠的就是ApplicationMaster。



## 核心概念

**1. Executor与Core**



**2. 并行度(Parallelism)**

整个集群并行执行任务的数量

**3. 有向无环图(DAG)**



# 核心编程

Spark计算框架为了能进行高并发和高吞吐的数据处理, 封装了三大数据结构:

- RDD: 弹性分布式数据集
- 累加器: 分布式共享只写变量
- 广播变量: 分布式共享只读变量

## 1. RDD

RDD数据处理方式类似于IO流，也有装饰者模式

RDD的数据只有在调用collect方法时，才会真正执行业务逻辑操作，之前的封装全都是功能的扩展

RDD不保存数据， 但是IO可以临时保存一部分数据

RDD就是逻辑的封装

### 1. RDD的创建

**1. 从内存中创建**

```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{SparkConf, SparkContext}

object RDD_Memory {
    def main(args: Array[String]): Unit = {
        // TODO 准备环境
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("RDD")
        val sc = new SparkContext(sparkConf)
        // TODO 创建RDD
        //从内存中创建RDD, 将内存中集合的数据作为处理的数据源
        val seq = Seq(1, 2, 3, 4)
        //parallelize : 并行
        val rdd: RDD[Int] = sc.parallelize(seq)

        rdd.collect().foreach(println)
        // TODO 关闭环境
        sc.stop();
    }
}
```

![image-20221020144720289](Spark%20image/image-20221020144720289.png)

parallelize方法不好理解,不好记忆,所以用makeRDD方法代替

```scala
		//parallelize : 并行
        //val rdd: RDD[Int] = sc.parallelize(seq)
        //makeRDD方法在底层实现时其实就是调用了rdd对象的parallelize方法
        val rdd: RDD[Int] = sc.makeRDD(seq)

        rdd.collect().foreach(println);
```

**2. 从文件中创建**

```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{SparkConf, SparkContext}

object RDD_File {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("RDD")
        val sc = new SparkContext(sparkConf)
        // TODO 创建RDD
        //从文件中创建RDD, 将文件中的数据作为处理的数据源
        //path路径的默认以当前的环境的根路径为基准,可以写相对路径,也可以写绝对路径
        //val rdd: RDD[String] = sc.textFile("datas/1.txt")
        //path路径可以是文件的具体路径,也可以时目录名称
        //val rdd: RDD[String] = sc.textFile("datas")
        //path路径还可以使用通配符
        val rdd: RDD[String] = sc.textFile("datas/1*.txt")
        //path还可以是分布式存储的路径
        rdd.collect().foreach(println)
        
        //TODO 关闭环境
        sc.stop()
    }
}
```

要知道数据是从那个文件中来,则要用到另一个方法: 

```scala
		//从文件中创建RDD, 将文件中的数据作为处理的数据源
        //textFile: 以行为单位来读取数据, 读取的结果都是字符串
        //wholeTextFiles: 以文件为单位读取数据
        //      读取的结果表示为元组: 第一个元素表示文件路径, 第二个元素表示文件内容
        val rdd: RDD[(String, String)] = sc.wholeTextFiles("datas")
        //path还可以是分布式存储的路径
        rdd.collect().foreach(println)
```

![image-20221020150851562](Spark%20image/image-20221020150851562.png)



### 2. RDD并行度和分区

设置分区数量

```Scala
import org.apache.spark.{SparkConf, SparkContext}

object RDD_Memory_Par {
    def main(args: Array[String]): Unit = {
        // TODO 准备环境
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("RDD")
        val sc = new SparkContext(sparkConf)
        // TODO 创建RDD
        //RDD的并行度 & 分区
        //makeRDD方法可以传递第二个参数, 这个参数表示分区的数量
        val rdd = sc.makeRDD(
            List(1, 2, 3, 4), 2
        )
        //将处理的数据保存成分区文件
        rdd.saveAsTextFile("output")
        //TODO 关闭环境
        sc.stop()
    }
}
```

![image-20221020151740776](Spark%20image/image-20221020151740776.png)



当不传递分区数量的时候

makeRDD()方法 第二个参数可以不传递, 那么makeRDD使用默认值: defaultParallelism(默认并行度)

`scheduler.conf.getInt("spark.default.parallelism", totalCores)`

spark在默认情况下, 从配置对象中获取配置参数: `spark.default.parallelism`

如果获取不到, 那么使用totalCores属性, 这个属性取值为当前运行环境的最大可用核数

```scala
		//  第二个参数可以不传递, 那么makeRDD使用默认值: defaultParallelism(默认并行度)
        val rdd = sc.makeRDD(List(1, 2, 3, 4))
```

![image-20221020151953011](Spark%20image/image-20221020151953011.png)

![image-20221020152605947](Spark%20image/image-20221020152605947.png)

`spark.default.parallelism`是可以配置的

```scala
		sparkConf.set("spark.default.parallelism", "3")
```

![image-20221020152919488](Spark%20image/image-20221020152919488.png)

数据是如何存放, 如何分区的

先将数据切分成Array, 将传过来的数据的长度和分区的数量进行如下计算, 得到元组的集合, 然后map进行切分, 根据得到的元组集合进行切分, 左闭右开地切分

```scala
def position(length: Long, numSlices: Int): Iterator[(Int, Int)] = {
    (0 until numSclices).iterator.map{ i => 
      val start = ((i * length) / numSlices).toInt
      val end = (((i + 1) * length) / numSlices).toInt
      (start, end)
    }
}
```

```Scala
import org.apache.spark.{SparkConf, SparkContext}

object RDD_File_Par {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("RDD")
        val sc = new SparkContext(sparkConf)

        //textFile可以将文件作为数据处理的数据源, 默认也可以设定分区
        // minPartitions : 最小分区数量
        // math.min(defaultPatitionsm, 2)
//        val rdd = sc.textFile("datas/1.txt")
        //如果不想使用默认地分区数量, 可以通过第二个参数指定分区数量
        val rdd = sc.textFile("datas/1.txt", 3)
        rdd.saveAsTextFile("output")
        sc.stop()
    }
}
```

数据分区地分配

1. 数据以行为单位进行读取

   spark读取文件,采用地是Hadoop地方式读取, 所以一行一行地读取, 和字节数没有关系

2. 数据读取时, 以偏移量为单位  偏移量不会被重复读取

   > 1@@		=> 012
   >
   > 2@@		=>345
   >
   > 3				=>6

3. 数据分区地偏移量范围的计算

   > 0	=> [0, 3]	=> 12
   >
   > 1	=> [3, 6]	=>3
   >
   > 2	=> [6, 7]	=>



**案例分析**

```scala
import org.apache.spark.{SparkConf, SparkContext}

object RDD_File_Par2 {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("RDD")
        val sc = new SparkContext(sparkConf)

        //14 byte / 2 = 7byte
        // 14 / 7 = 2(分区)
        val rdd = sc.textFile("datas/word.txt", 2)
        rdd.saveAsTextFile("output")

        sc.stop()
    }
}

		/*
        word.txt:
        1234567@@   => 012345678
        89@@        => 9 10 11 12
        0           => 13
        输出地分区中的数据:
        [0, 7]  => 1234567
        [7, 14] => 89
                   0
         */
//如果数据源为多个文件,那么计算分区时以文件为单位进行分区
```

![image-20221020162716251](Spark%20image/image-20221020162716251.png)

### 3. RDD的转换算子

**功能的补充和封装, 将旧的RDD包装成新的RDD**

RDD根据数据处理方式的不同将算子整体上分为Value类型, 双Value类型,和Key-Value类型

#### Value类型

##### 1. map

转换映射

```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{SparkConf, SparkContext}

object Operator_Transform {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        //TODO 算子 - map
        val rdd = sc.makeRDD(List(1, 2, 3, 4))
        // 1, 2, 3, 4 => 2, 4, 6, 8
        //转换函数
        def mapFunction(num: Int): Int ={
            num * 2
        }

        val mapRDD: RDD[Int] = rdd.map(mapFunction)
        mapRDD.collect().foreach(println)

        sc.stop()
    }
}
```

![image-20221021103014877](Spark%20image/image-20221021103014877.png)

在Scala中,我们不关心方法的名称,只关心结果,所以转换函数可以写成匿名函数, 结果是一样的

```scala
		val mapRDD: RDD[Int] = rdd.map((num: Int) => {num * 2})
```

根据scala的至简原则, 可以写成如下形式

```scala
		val mapRDD: RDD[Int] = rdd.map(_ * 2)
```

<font color="blue">小功能: 从服务器日志数据apache.log中获取用户请求URL资源路径</font>

![image-20221021103927574](Spark%20image/image-20221021103927574.png)

```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{SparkConf, SparkContext}

object Operator_Transform_Test {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        val rdd = sc.textFile("datas/apache.log")
        //长的字符串转换成短的字符串
        val mapRDD: RDD[String] = rdd.map(
            line => {
                val datas = line.split(" ")
                datas(6)
            }
        )
        mapRDD.collect().foreach(println)

        sc.stop()
    }
}
```

![image-20221021104620507](Spark%20image/image-20221021104620507.png)



如何体现RDD的并行计算

```scala
import org.apache.spark.{SparkConf, SparkContext}

object Operator_Transform_Par {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        //1. rdd的计算 一个分区内的数据是一个一个执行逻辑
        //      只有前面的一个数据全部执行完毕之后, 才会执行下一个数据
        //      分区内数据的执行是有序的
        val rdd = sc.makeRDD(List(1, 2, 3, 4))
        val mapRDD = rdd.map(
            num => {
                println(">>>>>>>>" + num)
                num
            }
        )
        val mapRDD1 = mapRDD.map(
            num => {
                println("#######" + num)
                num
            }
        )
        mapRDD1.collect()

        sc.stop()
    }
}
```

![image-20221021111201184](Spark%20image/image-20221021111201184.png)

执行的结果是没有规律的(这里可能是因为我的电脑的配置比较差, 所以没有规律没有很好体现出来)

现在我们给程序加上分区

```scala
		val rdd = sc.makeRDD(List(1, 2, 3, 4), 1)
```

![image-20221021111412150](Spark%20image/image-20221021111412150.png)

```
1. rdd的计算 一个分区内的数据是一个一个执行逻辑
      只有前面的一个数据全部执行完毕之后, 才会执行下一个数据
      分区内数据的执行是有序的
```

```scala
		val rdd = sc.makeRDD(List(1, 2, 3, 4), 2)
```

(我这里没有很好的体现出来)

```
2. 不同分区之间数据计算是无序的
```

##### 2. mapPartititons

以上的方法性能比较差, 现有一个提高性能的方法mapPartitions:

可以以分区为单位进行数据转换操作

但是会将整个分区的数据加载到内存进行引用

如果处理完的数据是不会被释放掉, 存在对象的引用

在内存较小, 数据量较大的场合下, 容易出现内存溢出, 这时使用map更好

```scala
import org.apache.spark.{SparkConf, SparkContext}

object Operator_Transform2 {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        val rdd = sc.makeRDD(List(1, 2, 3, 4), 2)
        val mapRDD = rdd.mapPartitions(
            iter => {
                println(">>>>>>>>>>>>")
                iter.map(_ * 2)
            }
        )
        mapRDD.collect().foreach(println)

        sc.stop()
    }
}
```

![image-20221021112603530](Spark%20image/image-20221021112603530.png)

<font color="blue">小功能: 获取每个数据分区的最大值</font>

```scala
import org.apache.spark.{SparkConf, SparkContext}

object Operator_Transform2_Test {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        val rdd = sc.makeRDD(List(1, 2, 3, 4), 2)
        val mapRDD = rdd.mapPartitions(
            iter => {
                List(iter.max).iterator
            }
        )
        mapRDD.collect().foreach(println)

        sc.stop()
    }
}
```

![image-20221021113101054](Spark%20image/image-20221021113101054.png)

<font color="red">map和mapPartitions的区别</font>

- 数据处理角度

  map算子是分区内一个数据一个数据的执行, 类似于串行操作

  而mapPartitions算子是以分区为单位进行批处理操作

- 功能角度

  map主要目的是将数据源中的数据进行转换和改变, 但不会减少或增多数据

  mapPartitions传递一个迭代器,返回一个迭代器, 没有要求元素的个数保持不变, 所以可以增加或减少

- 性能角度

  map因为类似于穿行操作, 所以性能比较低

  mapPartitions类似于批处理,所以性能比较高, 但会长时间占用内存, 可能会导致内存溢出, 这时推荐使用map

##### 3. mapParititionsWithIndex

增加了分区编号,即分区索引

将待处理的数据以分区为单位发送到计算节点进行处理,这里的处理是指可以进行任意地处理,哪怕是过滤数据,在处理同时可以获取当前分区索引

```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{SparkConf, SparkContext}

object Operator_Transform3 {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        //把第二个分区的数据保留
        val rdd = sc.makeRDD(List(1, 2, 3, 4), 2)
        val mapRDD: RDD[Int] = rdd.mapPartitionsWithIndex(
            (index, iter) => {
                if (index == 1) {
                    iter
                } else {
                    //空的迭代器
                    Nil.iterator
                }
            }
        )
        mapRDD.collect().foreach(println)

        sc.stop()
    }
}
```

![image-20221021142801517](Spark%20image/image-20221021142801517.png)

<font color="blue">小功能: 知道每个数据所在的分区</font>

```scala
		//知道每个数据所在的分区
        val rdd = sc.makeRDD(List(1, 2, 3, 4))
        val mapRDD = rdd.mapPartitionsWithIndex(
            (index, iter) => {
                iter.map(
                    num => {
                        (index, num)
                    }
                )
            }
        )
        mapRDD.collect().foreach(println)
```

![image-20221021143249634](Spark%20image/image-20221021143249634.png)

##### 4. flatMap

扁平映射

```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{SparkConf, SparkContext}

object Operator_Transform4 {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        val rdd: RDD[List[Int]] = sc.makeRDD(List(List(1, 2), List(3, 4)))
        val flatRDD: RDD[Int] = rdd.flatMap(
            list => {
                list
            }
        )
        flatRDD.collect().foreach(println)

        sc.stop()
    }
}
```

![image-20221021143732973](Spark%20image/image-20221021143732973.png)

案例: 对字符串进行扁平化操作

```scala
		val rdd: RDD[String] = sc.makeRDD(List(
            "hello spark", "hello scala"
        ))
        val flatRDD: RDD[String] = rdd.flatMap(
            s => {
                s.split(" ")
            }
        )

        flatRDD.collect().foreach(println)
```

![image-20221021144243121](Spark%20image/image-20221021144243121.png)

<font color="blue">小功能: 对List(List(1, 2), 3, List(4, 5))进行扁平化操作</font>

```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{SparkConf, SparkContext}

object Operator_Transform4_Test2 {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        val rdd = sc.makeRDD(List(
            List(1, 2), 3, List(4, 5)
        ))
        val flatRDD: RDD[Any] = rdd.flatMap {
            case list: List[_] => list  //如果是list集合就取出元素
            case dat => List(dat)  //如果不是list集合就变成List
        }

        flatRDD.collect().foreach(println)

        sc.stop()
    }
}
```

![image-20221021145023305](Spark%20image/image-20221021145023305.png)

##### 5. glom

将同一个分区的数据直接转换为相同类型的内存数组进行处理, <font color="red">分区不变</font>

```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{SparkConf, SparkContext}

object Operator_Transform5 {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        val rdd: RDD[Int] = sc.makeRDD(List(1, 2, 3, 4), 2)
        //Int => Array
        val glomRDD: RDD[Array[Int]] = rdd.glom()
        glomRDD.collect().foreach(data => println(data.mkString(",")))

        sc.stop()
    }
}
```

![image-20221021151044997](Spark%20image/image-20221021151044997.png)

<font color="blue">小功能: 计算所有分区最大值求和(分区内取最大值, 分区间最大值求和)</font>

```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{SparkConf, SparkContext}

object Operator_Transform5_Test {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        val rdd: RDD[Int] = sc.makeRDD(List(1, 2, 3, 4), 2)
        //Int => Array
        val glomRDD: RDD[Array[Int]] = rdd.glom()
        val maxRDD: RDD[Int] = glomRDD.map(
            array => {
                array.max
            }
        )
        println(maxRDD.collect().sum)

        sc.stop()
    }
}
```

![image-20221021151614154](Spark%20image/image-20221021151614154.png)

##### 5. groupBy

将函数根据指定的规则进行分组, 分区默认不变, 但是数据会被打乱重新组合, 我们将这样的操作称之为shuffle , 极限情况下, 数据可能会被分在同一个分区中

<font color="red">一个组的数据在一个分区中,但是并不是说一个分区中只有一个组</font>

```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{SparkConf, SparkContext}

object Operator_Transform6 {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        val rdd: RDD[Int] = sc.makeRDD(List(1, 2, 3, 4), 2)
        //groupBy会将数据源中的每一个数据进行分组,根据返回的分组key进行分组
        //相同的key值的数据会放在同一个组中
        //奇偶数进行分组
        def groupFunction(num: Int): Int = {
            num % 2
        }

        val groupRDD: RDD[(Int, Iterable[Int])] = rdd.groupBy(groupFunction)
        groupRDD.collect().foreach(println)

        sc.stop()
    }
}
```

![image-20221021153101524](Spark%20image/image-20221021153101524.png)

根据单词的首字母进行分组

```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{SparkConf, SparkContext}

object Operator_Transform6_Test {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        val rdd = sc.makeRDD(List("Hello", "Spark", "Scala", "Hadoop"), 2)
        //根据单词的首字母进行分组
        val groupRDD: RDD[(Char, Iterable[String])] = rdd.groupBy(_.charAt(0))
        groupRDD.collect().foreach(println)

        sc.stop()
    }
}
```

![image-20221021153502671](Spark%20image/image-20221021153502671.png)

**分组和分区没有必然的关系**

<font color="blue">小功能: 从服务器日志数据apache.log中获取每个时间段访问量</font>

```scala
import java.text.SimpleDateFormat
import java.util.Date

import org.apache.spark.rdd.RDD
import org.apache.spark.{SparkConf, SparkContext}

object Operator_Transform6_Test2 {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        val rdd = sc.textFile("datas/apache.log")
        val timeRDD: RDD[(String, Iterable[(String, Int)])] = rdd.map(
            line => {
                val datas = line.split(" ")
                val time = datas(3)
                val sdf = new SimpleDateFormat("dd/MM/yyyy:HH:mm:ss")
                val date: Date = sdf.parse(time)
                val sdf1 = new SimpleDateFormat("HH")
                val hour: String = sdf1.format(date)
                (hour, 1)
            }
        ).groupBy(_._1)
        timeRDD.map{
            case (hour, iter) =>
                (hour, iter.size)
        }.collect.foreach(println)

        sc.stop()
    }
}
```

![image-20221021155500211](Spark%20image/image-20221021155500211.png)

<font color="blue">小功能: WordCount</font>



##### 7. filter

将数据根据指定的规则进行筛选, 符合规则的数据保留, 不符合规则的数据丢弃

当数据进行筛选过滤后,分区不变, 但分区内的数据可能不均衡, 生产环境下, 可能会出现数据倾斜(数据不均衡)

```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{SparkConf, SparkContext}

object Operator_Transform7 {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        val rdd: RDD[Int] = sc.makeRDD(List(1, 2, 3, 4))
        //过滤出奇数
        val filterRDD: RDD[Int] = rdd.filter(num => num % 2 == 1)
        filterRDD.collect().foreach(println)

        sc.stop()
    }
}
```

![image-20221021160615154](Spark%20image/image-20221021160615154.png)

<font color="blue">小功能: 从服务器日志数据apache.log中获取2015年5月17日的请求路径</font>

```scala
import org.apache.spark.{SparkConf, SparkContext}

object Operator_Transform7_Test {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        val rdd = sc.textFile("datas/apache.log")
        rdd.filter(
            line => {
                val datas = line.split(" ")
                val time = datas(3)
                time.startsWith("17/05/2015")
            }
        ).collect().foreach(println)


        sc.stop()
    }
}
```

![image-20221021161357530](Spark%20image/image-20221021161357530.png)



##### 8. sample

根据指定的规则从数据集中抽取数据

```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{SparkConf, SparkContext}

object Operator_Transform8 {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        val rdd: RDD[Int] = sc.makeRDD(List(1, 2, 3, 4, 5, 6, 7, 8, 9, 10))
        //sample算子需要传递三个参数:
        //1. 第一个参数表示, 抽取数据后是否将数据返回 true(放回), false(丢弃)
        //2. 第二个参数表示,
        //                  如果抽取不放回的场合: 数据源中每条数据被抽取的概率,基准值的概念
        //                  如果抽取放回的场合: 表示数据源中每条数据被抽取的次数
        //3. 第三个参数表示, 抽取数据时随机算法的种子(当种子确定之后, 随机数就已经确定了)
        //                  如果不传递第三个参数, 那么使用的是当前系统时间
        println(rdd.sample(
            false,
            0.4,
            1
        ).collect().mkString(","))

        sc.stop()
    }
}
```

不论执行几次, 结果都是1, 2, 3

![image-20221022104032609](Spark%20image/image-20221022104032609.png)

现在我们把1去掉

```scala
		println(rdd.sample(
            false,
            0.4
        ).collect().mkString(","))
```

多执行几次, 结果是不一样的

![image-20221022104219777](Spark%20image/image-20221022104219777.png)

![image-20221022104245060](Spark%20image/image-20221022104245060.png)

现在把false改成true

```scala
		println(rdd.sample(
            withReplacement = true,
            2   //数据可能被抽取的次数是2次
        ).collect().mkString(","))
```

![image-20221022104959632](Spark%20image/image-20221022104959632.png)

该算子可以在数据倾斜的时候使用

分区的数据是均衡的, 而在shuffle的时候可能出现数据倾斜, sample可以判断数据是怎么倾斜的



##### 9. distinct

数据去重

```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{SparkConf, SparkContext}

object Operator_Transform9 {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        val rdd: RDD[Int] = sc.makeRDD(List(1, 2, 3, 4, 1, 2, 3, 4))
        val rdd1: RDD[Int] = rdd.distinct()
        rdd1.collect().foreach(println)

        sc.stop()
    }
}
```

![image-20221022110023695](Spark%20image/image-20221022110023695.png)

底层原理是分布式去重

##### 10. coalesce

根据数据量缩减分区, 用于大数据集过滤后, 提高小数据集的执行效率

```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{SparkConf, SparkContext}

object Operator_Transform10 {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        val rdd: RDD[Int] = sc.makeRDD(List(1, 2, 3, 4, 5, 6), 3)
        //coalesce算子默认情况下不会将分区的数据打乱重新组合
        //这种情况下的缩减分区可能会导致数据不均衡, 出现数据倾斜
        //如果想要让数据均衡, 可以进行shuffle处理
        val newRDD: RDD[Int] = rdd.coalesce(2)
        newRDD.saveAsTextFile("output")

        sc.stop()
    }
}
```

![image-20221022111438195](Spark%20image/image-20221022111438195.png)

coalesce算子默认情况下不会将分区的数据打乱重新组合
这种情况下的缩减分区可能会导致数据不均衡, 出现数据倾斜
如果想要让数据均衡, 可以进行shuffle处理

```scala
val newRDD: RDD[Int] = rdd.coalesce(2, shuffle = true)
```

![image-20221022111621501](Spark%20image/image-20221022111621501.png)

##### 11. repartition

扩大分区

```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{SparkConf, SparkContext}

object Operator_Transform11 {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        val rdd: RDD[Int] = sc.makeRDD(List(1, 2, 3, 4, 5, 6), 2)
        //coalesce算子是可以扩大分区的,但是如果不进行shuffle操作,是没有意义的, 不起作用
        //spark提供了一个简化的操作,
        //缩减分区: coalesce, 如果想要数据均衡, 可以采用shuffle
        //扩大分区: repartition, 底层代码调用的就是coalesce,而且肯定采用shuffle
        val newRDD: RDD[Int] = rdd.repartition(2)
        newRDD.saveAsTextFile("output")

        sc.stop()
    }
}
```

![image-20221022112535631](Spark%20image/image-20221022112535631.png)

##### 12. sortBy

根据指定的规则对数据进行排序

```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{SparkConf, SparkContext}

object Operator_Transform12 {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        val rdd: RDD[Int] = sc.makeRDD(List(6, 2, 4, 5, 3, 1), 2)
        val newRDD: RDD[Int] = rdd.sortBy(num => num)
        newRDD.saveAsTextFile("output")

        sc.stop()
    }
}
```

![image-20221022113054209](Spark%20image/image-20221022113054209.png)

```scala
import org.apache.spark.{SparkConf, SparkContext}

object Operator_Transform12_Test {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        val rdd = sc.makeRDD(List(("1", 1), ("11", 2), ("2", 3)), 2)
        //sortBy可以根据指定的规则对数据源中的数据进行排序, 默认为升序,第二个参数可以改变排序的方式
        //sortBy默认情况下不会改变分区, 但是中间存在shuffle操作
        val newRDD = rdd.sortBy(t => t._1, ascending = false)
        newRDD.collect().foreach(println)

        sc.stop()
    }
}
```

![image-20221022113820687](Spark%20image/image-20221022113820687.png)

#### 双Value类型

##### 13. intersection

对源RDD和参数RDD求交集后返回一个新的RDD

交集, 并集, 差集都要求两个数据源的类型要保持一致

##### 14. union

并集

##### 15. subtract

差集

##### 16. zip

拉链

拉链操作两个数据源的类型可以不一致

<font color="red">两个数据源的分区数量保持一致, 每个分区中的数据数量也要保持一致</font>

```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{SparkConf, SparkContext}

object Operator_Transform13 {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        val rdd1: RDD[Int] = sc.makeRDD(List(1, 2, 3, 4))
        val rdd2: RDD[Int] = sc.makeRDD(List(3, 4, 5, 6))
        //交集
        val rdd3: RDD[Int] = rdd1.intersection(rdd2)
        println(rdd3.collect().mkString(","))
        //并集
        val rdd4: RDD[Int] = rdd1.union(rdd2)
        println(rdd4.collect().mkString(","))
        //差集
        val rdd5: RDD[Int] = rdd1.subtract(rdd2)
        println(rdd5.collect().mkString(","))
        val rdd6: RDD[Int] = rdd2.subtract(rdd1)
        println(rdd6.collect().mkString(","))
        //拉链
        val rdd7: RDD[(Int, Int)] = rdd1.zip(rdd2)
        println(rdd7.collect().mkString(","))

        sc.stop()
    }
}
```

![image-20221022115059923](Spark%20image/image-20221022115059923.png)

#### Key - Value类型

##### 17 partitionBy

将数据按照指定Partitioner重新进行分区

```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{HashPartitioner, SparkConf, SparkContext}

object Operator_Transform14 {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        //TODO 算子 - (Key - Value类型)
        val rdd: RDD[Int] = sc.makeRDD(List(1, 2, 3, 4), 2)
        val mapRDD = rdd.map((_, 1))
        //隐式转换(二次编译)
        //partitionBy根据指定的分区规则对数据进行重分区
        mapRDD.partitionBy(new HashPartitioner(2)).saveAsTextFile("output")

        sc.stop()
    }
}
```

![image-20221022152706169](Spark%20image/image-20221022152706169.png)

问题: 

**1. 如果重分区的分区器和当前RDD的分区器一样怎么办**

一样: 类型, 分区数量都一样

如果一样的分区器那么使用分区器就没有任何意义

**2. spark的其他分区器**

- HashPartitioner
- RangePartitioner: 一般排序的时候使用
- PythonPartitioner : 包访问权限的Partitioner

**3. 如何按照自己的方法进行数据分区**

模仿HashPartitioner写一个自己的分区器



##### 18. reduceByKey:sweat_smile:

可以将数据按照相同的Key对Value进行聚合

```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{HashPartitioner, SparkConf, SparkContext}

object Operator_Transform15 {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        //TODO 算子 - (Key - Value类型)
        val rdd = sc.makeRDD(List(
            ("a", 1),("a", 2), ("a", 3), ("b", 4)
        ))
        //reduceByKey: 相同的key的数据进行value数据的聚合操作
        //scala语言中一般的聚合操作都是亮亮聚合, spark基于scala开发的, 所以他的聚合也是两两聚合
        //reduceByKey 中如果key的数据只有一个, 是不会参与计算的
        val reduceRDD: RDD[(String, Int)] = rdd.reduceByKey((x: Int, y: Int) => {
            println(s"x = ${x}, y = ${y}")
            x + y
        })
        reduceRDD.collect().foreach(println)

        sc.stop()
    }
}
```

![image-20221022154932278](Spark%20image/image-20221022154932278.png)

##### 19. groupByKey

将分区的数据直接转换为相同类型的内存数组进行后续处理

```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{SparkConf, SparkContext}

object Operator_Transform16 {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        //TODO 算子 - (Key - Value类型)
        val rdd = sc.makeRDD(List(
            ("a", 1),("a", 2), ("a", 3), ("b", 4)
        ))
        //groupByKey : 将数据源中的数据, 相同key的数据分在一个组中, 形成一个对偶元组
        //              元组中的第一个元素是key
        //              元组中的第二个元素是相同key的value集合
        val groupRDD: RDD[(String, Iterable[Int])] = rdd.groupByKey()
        groupRDD.collect().foreach(println)

        val groupRDD1: RDD[(String, Iterable[(String, Int)])] = rdd.groupBy(_._1)
        groupRDD1.collect().foreach(println)

        sc.stop()
    }
}
```

![image-20221022155648562](Spark%20image/image-20221022155648562.png)

**groupByKey与reduceByKey的区别**

groupByKey会导致数据打乱重组, 存在shuffle操作

spark中,shuffle操作必须罗盘处理, 不能再内存中数据等待, 会导致内存溢出. shuffle操作的性能非常低

reduceByKey支持分区内欲聚合功能, 可以有效减少shuffle时落盘的数据量

- 从shuffle角度: 两者都存在shuffle操作, 但是reduceByKey可以再shuffle前对分区内相同的key进行欲聚合(combine)功能, 这样会减少落盘的数据量, 而groupByKey只是进行分组, 不存在数据量减少的问题, reduceByKey性能比较高
- 从功能角度: reduceByKey包含分组聚合, groupByKey只能分组,不能聚合



reduceByKey 分区内和分区键计算规则是相同的

##### 20. aggregateByKey

将数据根据不同的规则进行分区内计算和分区间计算

```scala
import org.apache.spark.{SparkConf, SparkContext}

object Operator_Transform17 {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        //TODO 算子 - (Key - Value类型)
        val rdd = sc.makeRDD(List(
            ("a", 1),("a", 2), ("a", 3), ("a", 4)
        ), 2)
        //(a, [1, 2]) (a [3, 4])
        //分区内取最大值 : (a, 2), (a, 4)
        //分区间求和: (a, 6)

        //aggregateByKey存在函数柯里化, 有两个参数列表
        // 第一个参数列表: 需要传递一个参数, 表示为初始值
        //      主要用于当碰见第一个key时, 和value进行分区内计算
        // 第二个参数列表需要传递两个参数
        //      第一个参数表示分区内计算规则
        //      第二个参数表示分区间计算规则
        rdd.aggregateByKey(0)(
            (x, y) => math.max(x, y),
            (x, y) => x + y
        ).collect().foreach(println)

        sc.stop()
    }
}
```

![image-20221024104708485](Spark%20image/image-20221024104708485.png)

小练习: 获取相同key数据的平均值

```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{SparkConf, SparkContext}

object Operator_Transform17_Test {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        //TODO 算子 - (Key - Value类型)
        val rdd = sc.makeRDD(List(
            ("a", 1), ("a", 2), ("b", 3),
            ("b", 4), ("b", 5), ("a", 6)
        ), 2)

        //aggregateByKey 最终的返回数据结果应该和初始值类型保持一致
//        val value: RDD[(String, Int)] = rdd.aggregateByKey(0)(_ + _, _ + _)
//        value.collect().foreach(println)

        //获取相同key的数据的平均值 => (a, 3), (b, 4)
        //第一个0 表示计算的初始值, 第二个0表示计算个数的初始值
        val newRDD: RDD[(String, (Int, Int))] = rdd.aggregateByKey((0, 0))(
            (t, v) => {
                (t._1 + v, t._2 + 1)
            },
            (t1, t2) => {
                (t1._1 + t2._1, t1._2 + t2._2)
            }
        )
        val resultRDD: RDD[(String, Int)] = newRDD.mapValues {
            case (num, cnt) =>
                num / cnt
        }
        resultRDD.collect().foreach(println)

        sc.stop()
    }
}
```

![image-20221024112756371](Spark%20image/image-20221024112756371.png)



##### 21. foldByKey

当分区内和分区键的计算规则相同的时候, 可以使用foldByKey代替aggregateByKey

```scala
import org.apache.spark.{SparkConf, SparkContext}

object Operator_Transform18 {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        //TODO 算子 - (Key - Value类型)
        val rdd = sc.makeRDD(List(
            ("a", 1),("a", 2), ("a", 3), ("b", 4)
        ), 2)

//        rdd.aggregateByKey(0)(_ + _, _ + _).collect().foreach(println)
        //如果聚合计算时, 分区内和分区间计算规则相同, spark提供了简化的方法
        rdd.foldByKey(0)(_ + _).collect().foreach(println)

        sc.stop()
    }
}
```

![image-20221024111035258](Spark%20image/image-20221024111035258.png)

##### 22. combineByKey

把aggregateByKey中需要的初始值去掉, 直接把相同key的第一个数据进行结构转换, 实现操作

```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{SparkConf, SparkContext}

object Operator_Transform19 {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        //TODO 算子 - (Key - Value类型)
        val rdd = sc.makeRDD(List(
            ("a", 1), ("a", 2), ("b", 3),
            ("b", 4), ("b", 5), ("a", 6)
        ), 2)

        //combineByKey方法需要三个参数
        //第一个参数表示: 将相同的key的第一个数据进行结构的转换, 实现操作
        //第二个参数表示: 分区内的计算规则
        //第三个参数表示: 分区间的计算规则
        val newRDD: RDD[(String, (Int, Int))] = rdd.combineByKey(
            v => (v, 1),
            (t: (Int, Int), v) => {
                (t._1 + v, t._2 + 1)
            },
            (t1: (Int, Int), t2: (Int, Int)) => {
                (t1._1 + t2._1, t1._2 + t2._2)
            }
        )
        val resultRDD: RDD[(String, Int)] = newRDD.mapValues {
            case (num, cnt) =>
                num / cnt
        }
        resultRDD.collect().foreach(println)

        sc.stop()
    }
}
```

![image-20221024114625131](Spark%20image/image-20221024114625131.png)

##### 23 join

在类型为（K， V）和（K， W）的RDD上调用，返回一个相同key对应的所有元素连接在一起的（K，（V，W））的RDD

```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{SparkConf, SparkContext}

object Operator_Transform20 {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        //TODO 算子 - (Key - Value类型)
        val rdd1 = sc.makeRDD(List(
            ("a", 1), ("b", 2), ("c", 3), ("a", 8)
        ))
        val rdd2 = sc.makeRDD(List(
            ("a", 4), ("b", 5), ("c", 6),("d", 3)
        ))
        //join : 两个不同数据源的数据，相同的key的value会连接在一起， 形成元组
        //      如果两个数据源中key没有匹配上, 那么数据不会出现在结果中
        //      如果两个数据源中key有多个相同的, 会依次匹配,可能会出现笛卡尔乘积, 数据量会几何形增长,性能就会降低
        val joinRDD: RDD[(String, (Int, Int))] = rdd1.join(rdd2)
        joinRDD.collect().foreach(println)


        sc.stop()
    }
}
```

![image-20221024155618159](Spark%20image/image-20221024155618159.png)

##### 24. leftOuterJoin

```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{SparkConf, SparkContext}

object Operator_Transform21 {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        //TODO 算子 - (Key - Value类型)
        val rdd1 = sc.makeRDD(List(
            ("a", 1), ("b", 2), ("c", 3)
        ))
        val rdd2 = sc.makeRDD(List(
            ("a", 4), ("b", 5)
        ))
        val leftJoinRDD: RDD[(String, (Int, Option[Int]))] = rdd1.leftOuterJoin(rdd2)
        leftJoinRDD.collect().foreach(println)


        sc.stop()
    }
}
```

![image-20221024160014782](Spark%20image/image-20221024160014782-16665984166991.png)

##### 25. rightOuterJoin

```scala
		//TODO 算子 - (Key - Value类型)
        val rdd1 = sc.makeRDD(List(
            ("a", 1), ("b", 2)
        ))
        val rdd2 = sc.makeRDD(List(
            ("a", 4), ("b", 5), ("c", 3)
        ))

        val rightJoinRDD = rdd1.rightOuterJoin(rdd2)
        rightJoinRDD.collect().foreach(println)
```

![image-20221024160308221](Spark%20image/image-20221024160308221.png)

##### 26 cogroup

```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{SparkConf, SparkContext}

object Operator_Transform22 {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        //TODO 算子 - (Key - Value类型)
        val rdd1 = sc.makeRDD(List(
            ("a", 1), ("b", 2)
        ))
        val rdd2 = sc.makeRDD(List(
            ("a", 4), ("b", 5), ("c", 3), ("c", 7)
        ))
        // cogroup : connect + group
        //cogroup 最多可以连接三个
        val cgRDD: RDD[(String, (Iterable[Int], Iterable[Int]))] = rdd1.cogroup(rdd2)
        cgRDD.collect().foreach(println)


        sc.stop()
    }
}
```

![image-20221024160841949](Spark%20image/image-20221024160841949.png)

#### 案例实操

**1. 数据准备**

agent.log: 时间戳, 省份, 城市, 用户, 广告, 中间字段使用空格分割

![image-20221024161407356](Spark%20image/image-20221024161407356.png)

**2. 需求描述**

统计出每一个省份每个广告被点击数量排行的Top3

**3. 需求分析**

![image-20221024165400184](Spark%20image/image-20221024165400184.png)

**4. 功能实现**

```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{SparkConf, SparkContext, rdd}

object Request {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        //TODO 案例实操
        //1. 获取原始数据: 时间戳,省份,城市,用户,广告
        val rdd: RDD[String] = sc.textFile("datas/agent.log")
        //2. 将原始数据进行结构的转换, 方便统计
        //  时间戳, 省份, 城市, 用户, 广告 => ((省份, 广告), 1)
        val mapRDD: RDD[((String, String), Int)] = rdd.map(
            line => {
                val datas = line.split(" ")
                ((datas(1), datas(4)), 1)
            }
        )
        //3. 将转换结构后的数据,进行分组聚合
        //  ((省份, 广告), 1) => ((省份, 广告), sum)
//        val value1: RDD[((String, String), Int)] = mapRDD.groupBy(
//            _._1
//        ).map {
//            case (k, v) => (k, v.size)
//        }
        val reduceRDD: RDD[((String, String), Int)] = mapRDD.reduceByKey(_ + _)
        //4. 将聚合的结果进行结构的转换
        //  ((省份, 广告), sum) => (省份, (广告, sum))
        val newMapRDD: RDD[(String, (String, Int))] = reduceRDD.map {
            case ((pro, adv), sum) => (pro, (adv, sum))
        }
        //5. 将转换结构后的数据根据省份进行分组
        // (省份, [(广告A, sumA), (广告B, sumB)])
        val groupRDD: RDD[(String, Iterable[(String, Int)])] = newMapRDD.groupByKey()
        //6. 将分组后的数据组内排序(降序), 取前3名
        val result: RDD[(String, List[(String, Int)])] = groupRDD.mapValues(
            iter => {
                iter.toList.sortBy(_._2)(Ordering.Int.reverse).take(3)
            }
        )
        //7. 采集数据打印在控制台
        result.collect().foreach(println)

        sc.stop()
    }
}
```



### 4. RDD的行动算子

**触发任务的调度和作业的执行, 底层代码调用的是环境对象的runJob方法, 底层代码中会创建ActiveJob, 并提交执行**

#### 1. reduce

```scala
import org.apache.spark.{SparkConf, SparkContext}

object Action_Reduce {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        val rdd = sc.makeRDD(List(1, 2, 3, 4))
        //TODO 行动算子
        //reduce
        val i = rdd.reduce(_ + _)
        println(i)  //10

        sc.stop()
    }

}
```

#### 2. collect

```scala
		val rdd = sc.makeRDD(List(1, 2, 3, 4))
		//collect : 方法会将不同分区的数据按照分区顺序采集到Driver端内存中, 形成数组
        val ints: Array[Int] = rdd.collect()
        println(ints.mkString(",")) //1,2,3,4
```

#### 3. count

```scala
        val rdd = sc.makeRDD(List(1, 2, 3, 4))
		//count : 数据源中数据的个数
        val cnt: Long = rdd.count()
        println(cnt)    //4
```

#### 4. first

```scala
        val rdd = sc.makeRDD(List(1, 2, 3, 4))
		//first: 获取数据源中数据的第一个
        val first: Int = rdd.first()
        println(first)  //1
```

#### 5. take

```scala
        val rdd = sc.makeRDD(List(1, 2, 3, 4))
		//take : 获取n个数据
        val ints1 = rdd.take(3)
        println(ints1.mkString(","))    //1,2,3
```

#### 6. takeOrdered

```scala
		//takeOrdered : 数据排序后, 取n个数据
        val rdd2 = sc.makeRDD(List(4, 3, 2, 1))
        val ints2: Array[Int] = rdd2.takeOrdered(3)
        println(ints2.mkString(","))    //1,2,3
```

#### 7. aggregate

分区的数据通过初始值和分区内的数据进行聚合, 然后再和初始值进行分区间的数据聚合

```scala
import org.apache.spark.{SparkConf, SparkContext}

object Action2_Aggregate {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        val rdd = sc.makeRDD(List(1, 2, 3, 4), 2)
        //TODO 行动算子
        //aggregateByKey : 初始值只会参与分区内计算
        //aggregate初始值会参与分区内计算, 并且参与分区间计算
        val result: Int = rdd.aggregate(10)(_ + _, _ + _)
        // 10 + 13 + 17 = 40
        println(result)

        sc.stop()
    }

}
```

![image-20221030153448198](Spark%20image/image-20221030153448198.png)

#### 8. fold

相当于简化版的aggregate, 当分区内的计算规则和分区间的计算规则相同的时候,可以使用这个方法



#### 9. countByKey

```scala
import org.apache.spark.{SparkConf, SparkContext}

object Action3_CountByKey {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

//        val rdd = sc.makeRDD(List(1, 2, 2, 4), 2)
//        //TODO 行动算子
//        val intToLong: collection.Map[Int, Long] = rdd.countByValue()
//        println(intToLong)  //Map(4 -> 1, 2 -> 2, 1 -> 1)

        val rdd = sc.makeRDD(List(
            ("a", 1), ("a", 1), ("a", 1)
        ))
        val stringToLong: collection.Map[String, Long] = rdd.countByKey()
        println(stringToLong)   //Map(a -> 3)

        sc.stop()
    }

}
```

##### wordCount相关实现

```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{SparkConf, SparkContext}

import scala.collection.mutable


object Spark04_WordCount {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("WordCount")
        val sc = new SparkContext(sparkConf)

        wordCount4(sc)

        sc.stop()
    }

    //核心方法: groupBy
    def wordCount1(sc : SparkContext) : Unit = {
        val rdd = sc.makeRDD(List("Hello Scala", "Hello Spark"))
        val words = rdd.flatMap(_.split(" "))
        val group: RDD[(String, Iterable[String])] = words.groupBy(word => word)
        val wordcount: RDD[(String, Int)] = group.mapValues(iter => iter.size)
    }

    //核心方法: groupByKey
    def wordCount2(sc : SparkContext) : Unit = {
        val rdd = sc.makeRDD(List("Hello Scala", "Hello Spark"))
        val words = rdd.flatMap(_.split(" "))
        val wordOne: RDD[(String, Int)] = words.map((_, 1))
        val group: RDD[(String, Iterable[Int])] = wordOne.groupByKey()
        val wordcount: RDD[(String, Int)] = group.mapValues(iter => iter.size)
    }

    //核心方法: reduceByKey
    def wordCount3(sc : SparkContext) : Unit = {
        val rdd = sc.makeRDD(List("Hello Scala", "Hello Spark"))
        val words = rdd.flatMap(_.split(" "))
        val wordOne: RDD[(String, Int)] = words.map((_, 1))
        val wordCont: RDD[(String, Int)] = wordOne.reduceByKey(_ + _)
    }

    //核心算法: aggregateByKey
    def wordCount4(sc : SparkContext) : Unit = {
        val rdd = sc.makeRDD(List("Hello Scala", "Hello Spark"))
        val words = rdd.flatMap(_.split(" "))
        val wordOne: RDD[(String, Int)] = words.map((_, 1))
        val wordCount: RDD[(String, Int)] = wordOne.aggregateByKey(0)(_ + _, _ + _)
    }

    //核心算法: foldByKey
    def wordCount5(sc : SparkContext) : Unit = {
        val rdd = sc.makeRDD(List("Hello Scala", "Hello Spark"))
        val words = rdd.flatMap(_.split(" "))
        val wordOne: RDD[(String, Int)] = words.map((_, 1))
        val wordCount: RDD[(String, Int)] = wordOne.foldByKey(0)(_ + _)
    }

    //核心方法: combineByKey
    def wordCount6(sc : SparkContext) : Unit = {
        val rdd = sc.makeRDD(List("Hello Scala", "Hello Spark"))
        val words = rdd.flatMap(_.split(" "))
        val wordOne: RDD[(String, Int)] = words.map((_, 1))
        val wordCount: RDD[(String, Int)] = wordOne.combineByKey(
            v=>v,
            (x: Int, y: Int) => x + y,
            (x: Int, y: Int) => x + y
        )
    }

    //核心方法: countByKey
    def wordCount7(sc : SparkContext) : Unit = {
        val rdd = sc.makeRDD(List("Hello Scala", "Hello Spark"))
        val words = rdd.flatMap(_.split(" "))
        val wordOne: RDD[(String, Int)] = words.map((_, 1))
        val wordCount: collection.Map[String, Long] = wordOne.countByKey()
    }

    //核心方法: countByValue
    def wordCount8(sc : SparkContext) : Unit = {
        val rdd = sc.makeRDD(List("Hello Scala", "Hello Spark"))
        val words = rdd.flatMap(_.split(" "))
        val wordCount: collection.Map[String, Long] = words.countByValue()
    }

    //核心方法: reduce(10 :aggregate 11 : fold)
    def wordCount9(sc : SparkContext) : Unit = {
        val rdd = sc.makeRDD(List("Hello Scala", "Hello Spark"))
        val words = rdd.flatMap(_.split(" "))

        //[(word, count), (word, count)]
        //word => Map[(word, 1)]
        val mapWord: RDD[mutable.Map[String, Long]] = words.map(
            word => {
                mutable.Map[String, Long]((word, 1))
            }
        )
        val wordCount: mutable.Map[String, Long] = mapWord.reduce(
            (map1, map2) => {
                map2.foreach{
                    case (word, count) => {
                    val newCount: Long = map1.getOrElse(word, 0L) + count
                    map1.update(word, newCount)
                    }
                }
                map1
            }
        )
        println(wordCount)
    }
}
```





#### 10 save相关

```scala
import org.apache.spark.{SparkConf, SparkContext}

object Action4_Save {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        //TODO 行动算子

        val rdd = sc.makeRDD(List(
            ("a", 1), ("a", 1), ("a", 1)
        ))
        rdd.saveAsTextFile("output")
        rdd.saveAsObjectFile("output1")
        //saveAsSequenceFile方法要求数据的格式必须为K-V类型
        rdd.saveAsSequenceFile("output2")

        sc.stop()
    }

}

```

####  11. foreach

分布式遍历RDD中的每一个元素, 调用指定函数

```scala
import org.apache.spark.{SparkConf, SparkContext}

object Action5_foreach {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Operator")
        val sc = new SparkContext(sparkConf)

        //TODO 行动算子

        val rdd = sc.makeRDD(List(1, 2, 3, 4))
        //先采集, 再循环
        //是Driver端内存集合的循环遍历方法
        rdd.collect().foreach(println)
        println("***************")
        //没有顺序的  是Executor端内存数据打印
        rdd.foreach(println)

        sc.stop()
    }
}
```

![image-20221030163641027](Spark%20image/image-20221030163641027.png)

```
算子: Operator(操作)
      RDD的方法和Scala集合对象的方法不一样
      集合对象的方法都是再同一个节点的内存中完成的
      RDD的方法可以将计算逻辑发送到Executor端(分布式系欸但)执行
      为了区分不同的处理效果, 所以将RDD的方法称之为算子
      RDD的方法外部的操作都是在Driver端执行的, 而方法内部的逻辑代码是在Executor端执行
```



### 5. 序列化

#### 1. 闭包检查

从计算的角度， <font color="red">算子以外的代码都是在Driver端执行，算子里面的代码都是在Executor端执行</font>。那么在Scala的函数式编程中，就会导致算子内经常会用到算子外的数据，这样就形成了闭包的效果，如果使用的算子外的数据无法序列化，就意味着无法传值给Executor端执行，就会发生错误，所以需要在执行任务计算之前，检测闭包内的对象是否可以进行序列化，这个操作称之为闭包检测

#### 2. 序列化方法和属性

```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{SparkConf, SparkContext}

object Serial01 {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Serial")
        val sc = new SparkContext(sparkConf)

        val rdd: RDD[String] = sc.makeRDD(Array("hello world", "hello spark", "hive", "atguigu"))
        val search = new Search("h")
        search.getMatch1(rdd).collect().foreach(println)
        search.getMatch2(rdd).collect().foreach(println)

        sc.stop()
    }
    //查询对象
    //类的构造参数其实是类的属性, 构造参数需要进行闭包检测，七十九等同于类进行闭包检测
    class Search(query: String) extends Serializable {
        def isMatch(s: String): Boolean = {
            s.contains(query)
        }
        //函数序列化案例
        def getMatch1(rdd: RDD[String]): RDD[String] = {
            rdd.filter(isMatch)
        }
        //属性序列化案例
        def getMatch2(rdd: RDD[String]): RDD[String] = {
            rdd.filter(x => x.contains(query))
        }
    }
}

```

### 6. RDD的依赖

相邻的两个RDD的关系称之为依赖关系

新的RDD依赖于旧的RDD

多个连续的RDD的依赖关系,称之为血缘关系

每个RDD会保存血缘关系

![image-20221101165434199](Spark%20image/image-20221101165434199.png)

加入其中某个依赖出现问题:

![image-20221101165622597](Spark%20image/image-20221101165622597.png)

### 7. 持久化

如果我们要对一组数据先进行聚合，然后再分组，如果我们写两部分代码就显得冗余，有很多重复的代码，如果我们将重复的代码删除，用同一个mapRDD，虽然宏观上可行，但是实际上前面一样的代码还是执行了两次，因为：

RDD中不存储数据

如果一个RDD需重复使用，那么需要从头再次执行来获取数据

RDD对象可以重用，但是数据无法重用

![image-20221103162524960](Spark%20image/image-20221103162524960.png)

所以我们将数据map过后进行持久化存储，将数据缓存到内存中，然后分别进行reduceByKey和groupByKey，这样我们的目的就达成了

**cache**

```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{SparkConf, SparkContext}

object persist01 {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Persist")
        val sc = new SparkContext(sparkConf)

        val list = List("Hello Scala", "Hello Spark")
        val rdd = sc.makeRDD(list)
        val flatRDD: RDD[String] = rdd.flatMap(_.split(" "))
        val mapRDD = flatRDD.map((_, 1))
        //将数据缓存到内存中
        mapRDD.cache()
        val reduceRDD: RDD[(String, Int)] = mapRDD.reduceByKey(_ + _)
        reduceRDD.collect().foreach(println)
        println("*****************************")
        val groupRDD: RDD[(String, Iterable[Int])] = mapRDD.groupByKey()
        groupRDD.collect().foreach(println)

        sc.stop()
    }
}
```

![image-20221103163335968](Spark%20image/image-20221103163335968.png)

**persist**

```scala
//cache默认持久化的操作，只能将数据保存到内存中，如果想要保存到磁盘文件，需要更改存储级别
//mapRDD.cache()

//持久化操作必须在行动算子执行时完成的
mapRDD.persist(StorageLevel.DISK_ONLY)
```

RDD通过Cache或者Persist方法将前面的计算结果缓存，默认情况下会把数据以缓存在JVM的堆内存中。但是并不是这两个方法被调用时立即缓存，而是触发后面的action算子时，该RDD将会被缓存在计算节点的内存中，并供后面重用

<font color="red">RDD对象的持久化操作不一定是为了重用, 在数据执行较长, </font>

**检查点**

```scala
sc.setCheckpointDir("cp")

//checkpoint需要落盘,需要指定检查点保存路径
//检查点路径保存的文件, 当作业执行完毕后, 不会被删除
//一般保存路径都是在分布式存储系统中: HDFS
mapRDD.checkpoint()
```



**区别**

1. cache: 将数据临时存储在内存中进行数据重用

   ​			会在血缘关系中添加新的依赖, 一旦出现问题, 可以重头读取数据

2. persist: 将数据临时存储在磁盘文件中进行数据重用

​					涉及到磁盘IO, 性能较低, 但是数据安全

​					如果作业执行完毕, 临时保存的数据文件就会丢失

3. checkpoint: 将数据长久的保存在磁盘文件中进行数据重用

​						涉及磁盘IO, 性能较低, 但是数据安全

​						为了保证数据安全, 一般情况下, 会独立执行作业

​						为了能够提高效率, 一般情况下, 是需要和cache联合使用

​						执行过程中会切断血缘关系, 重新建立新的血缘关系

​						checkpoint等同于改变数据源

### 8. 分区器

```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{Partitioner, SparkConf, SparkContext}

object part01 {
    def main(args: Array[String]): Unit = {
        val sparkConf = new SparkConf().setMaster("local[*]").setAppName("Partition")
        val sc = new SparkContext(sparkConf)

        val rdd = sc.makeRDD(List(
            ("nba", "xxxxxxxxx"),
            ("cba", "xxxxxxxxx"),
            ("wnba", "xxxxxxxxx"),
            ("nba", "xxxxxxxxx")
        ), 3)
        val partRDD: RDD[(String, String)] = rdd.partitionBy(new MyPartitioner)
        partRDD.saveAsTextFile("output")


        sc.stop()
    }

    /**
     * 自定义分区器:
     *  1. 继承Partitioner
     *  2. 重写方法
     */
    class MyPartitioner extends Partitioner{
        //分区数量
        override def numPartitions: Int = 3

        //根据数据的key值返回数据所在的分区索引(从0开始)
        override def getPartition(key: Any): Int = {
            key match{
                case "nba" => 0
                case "wnba" => 1
                case _ => 2
            }
        }
    }
}
```

![image-20221104173102547](Spark%20image/image-20221104173102547.png)

### 9. 文件读取与保存



# SparkSQL

## 1. spark SQL特点

1. 易整合

2. 统一的数据访问

3. 兼容Hive

4. 标准数据连接

   通过JDBC或者ODBC来连接

## 2. DataFrame

