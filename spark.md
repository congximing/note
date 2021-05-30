# 职业规划

字节跳动大数据工程师

数据库工程师

```
Hadoop，Spark，mapreduce, Hive，Flink
hbase，MySQL，redis 
trouble-shooting，设计模式
sqoop，kafka

7月秋招
```

# 学习规划

```
需求一的剩下几个方法还没有完 p114-p116，p122

22：127-152 4.37h 源码
24：185-210 3.40h	SparkStreaming

8:00-11:00 学习
13:00-17:00 学习
吃饭跑步
19:00-22:00 复习总结

5h spark
2h 算法
3h 复习写代码
```

# 一些好的习惯

```
画图
做好笔记，用来复习用，以免重复学习浪费时间！！！

```

# 算法

为什么学算法？

掌握一些基础的`“思维”`轮子，来更好的实现编码。

1.算法第四版

2.剑指offer

3.左2

# 勉励：

做别人做不到的事情，才能脱颖而出！！！



# spark

## RDD分区(内存与文件)

```
p30 RDD的五大核心：宁可移动计算，也不要移动分区的数据文件，网络io开销大。
p31 RDD的执行原理：从Driver中的TaskPool发送到Executor中执行。
p32 RDD的创建
p36 内存分区的规则
	numSlices
	length/numSlices
p38 1.文件分区数量的确定：
	goalSize=totalSize/min(parallelize,2)
	totalSize/goalSize
	如果totalSize%goalSize/goalSize>0.1,则分区数量+1。
	
	2.数据如何分到特定的分区：
	按照每个分区的字节数当作偏移量。例如分区的字节数为7，偏移量为7，[0,7],[7,14],按行读取
	
p40 RDD算子
	转换算子：封装	flatMap,map
	行动算子：触发任务 collect
    问题(初始)->算子->问题(审核)->算子->问题(完成)
```

## RDD转换算子

### 创建RDD

```scala
sc.map()
sc.mapPartitions

```

```scala
//p43 体现并行计算
//p44 mapPartitions
// 性能比较高，会缓存整个分区的数据，相当于批处理。
// 缺点：执行完的数据，不会被释放，可能导致内存泄漏。

//p60 sortBy，根据返回的值进行排序。底层有shuffle

//p62 拉链可以不同类型，交集、并集和差集必须同种类型。

//p67 groupByKey 存在shuffle阶段，数据不能放在内存，容易导致内存泄漏。
```

<img src="E:\github\spark\1.JPG" style="zoom:80%;" />

```scala
//p70 aggregateByKey 分区内和分区间的计算相同，则使用foldByKey
//初始类型(0,0)，通过每个key，进行操作
rdd.aggregateByKey(0)(_+_,_+_)
rdd.foldByKey(0)(_+_)
//p73 求平均值,使用combineByKey(),第一个参数让第一个key变形

//aggregateByKey 简化为 foldByKey
aggregateByKey
combineByKeyWithClassTag[U](
	(v:V)=>cleanedSeqOp(createZero(),v),
    cleanedSeqOp,
    combOp
)

foldByKey
combineByKeyWithClassTag[V](
	(v:V)=>cleanedFunc(createZero(),v),//初始值和分区内的操作
    cleanedFunc,
    cleanedFunc
)

//比较高效的聚合
reduceByKey
combineByKeyWithClassTag[V](
	(v:V)=>v,
    func,
    func
)

combineByKey
combineByKeyWithClassTag(
	createCombiner,//相同key第一个数据进行处理
    mergeValue,
    mergeCombiners
)
```

### flatMap

```scala
//计算：通过分割，返回数组
//输入：value	
//输出：Array 
//RDD: String
//"hello world" ==> "hello" "world"
val flatRDD: RDD[String] = rdd.flatMap(_.split(" "))
```

### glom

```scala
//计算：通过分割，返回数组
//输入： 	
//输出：  
//RDD: Array[String]
//"hello"  ==> ["hello"]
val glomRDD: RDD[Array[String]] = flatRDD.glom()
```

### groupBy

```scala
//计算：根据规则进行分组
//产生shuffle，占用大量内存
//输入：value	
//输出：value
//RDD: (Int, Iterable[Int])
// (0,CompactBuffer(2, 4))
// (1,CompactBuffer(1, 3, 5))
val rdd: RDD[Int] = sc.makeRDD(List(1, 2, 3, 4, 5))
val groupRDD: RDD[(Int, Iterable[Int])] = rdd.groupBy(_%2)
```

### groupByKey

```scala
//通过key分组
```



### filter

```scala
//计算：根据规则进行过滤，返回奇数
//输入：value	
//输出：value
//RDD: Int 
val filterRDD: RDD[Int] = sc.makeRDD(List(1, 2, 3, 4))
val rdd: RDD[Int] = filterRDD.filter(_%2!=0)
```

### sample

```scala
//随机抽取数据
```

### distinct

```scala
//数组去重
```

### 分区操作

#### coalesce

```scala
//缩减分区，不shuffle是无效的
```

#### repartition



### 集合

intersection 交集

union 并集

subtract 差集

zip 拉链

### cogroup

```scala
//connect+group
//(k,[(),(),()]) 相当于 一个键下的多个字段
```

### sortBy

```scala
//rdd与本地的区别
//计算：排序
//输入：value 排序字段
//输出：Array
//RDD: Int 
val actions: immutable.Seq[UserVisitAction]=iter.toList.sortBy(_._2)(Ordering.Int.reverse).take(10)
val resultRDD: Array[(String, (Int, Int, Int))] = analysisRDD.sortBy(_._2, false).take(10)
```

### sortByKey

```scala
//按照key排序
```



## 案例：每个省份前三的点击量

分析：

![](E:\github\spark\2.JPG)

```scala
//map()与map{}的区别：
rdd.map{//匹配模式
    case (hour,iter)=>{//变量
        (hour,iter.size)
    }
}

//p80 (省份,[(a,numA),(b,numB),(c,numC),......])，每个省份取前三
//计算，输入输出
groupRDD.mapValues(//以一个key为单位
	iter=>{
        iter.toList().sortBy(_._2)(Ordering.Int.reverse).take(3)
    }
)

//p81 行动算子：runJob()--submitJob()--post(JobSubmitted)--ActiveJob

//p85 8种wordcount

//p86 reduce wordcount
//in: [(w,c),(w,c),......] 两个map的合并
val res: mutable.Map[String, Long] = mapWord.reduce(
      (m1, m2) => {
        m2.foreach {
          case (w, c) => {
            val newCount = m1.getOrElse(w, 0L) + c
            m1.update(w, newCount)
          }
        }
        m1
      }
    )

//p88 foreach 两者的区别
rdd.collect().foreach(println)//driver
rdd.foreach(println)//executor
//RDD和Scala的方法区别：
//RDD将计算与数据剥离，可以作为数据传输到Executor端执行，而Scala方法不能传输

//p89 闭包，匿名函数将外部环境的变量引入到内部，形成闭包，此外还有闭包检验，确认是否序列化

//todo query是类的属性，因此需要序列化
  class Search(query:String)extends Serializable {
    def isMatch(s:String):Boolean= s.contains(query)

    //案例一
    def getMatch(rdd:RDD[String])=rdd.filter(isMatch) //todo query 是闭包
    //案例二
    def getMatch1(rdd:RDD[String])={
      val q=query //todo 改变闭包生命周期,不引用Search(query:String)的query
      rdd.filter(s=>s.contains(q))
    }
  }
```

## RDD依赖关系

```scala
//p92 血缘关系、依赖关系
```

<img src="E:\github\spark\3.JPG" style="zoom:80%;" />

```scala
//血缘关系
```

<img src="E:\github\spark\4.JPG" style="zoom:80%;" />

```scala
//oneToOne依赖 窄依赖
```

<img src="E:\github\spark\5.JPG" style="zoom:80%;" />

```scala
//shuffle依赖 宽依赖
```

<img src="E:\github\spark\6.JPG" style="zoom:80%;" />



```scala
//p95 宽依赖，由于会有shuffle阶段，所以要等一段时间进行分区（也叫阶段）
rdd.dependencies
//p97 job Stage=宽依赖+1 Task之间的关系，
//宽依赖需要进行shuffle，newRDD的不同分区，分区数=Task

```

## 持久化

```scala
//p100 持久化操作 ，可以为了重用，也可以保存重要的数据。
mapRDD.persist(StorageLevel.DISK_ONLY)

//p101 检查点 执行完不会被删除 hdfs://
rdd.setCheckPointDir()
rdd.checkPoint()

//p102 cache() presist() checkPoint()的区别
//checkPoint()会重新执行一次任务，来将数据持久化到文件，但是性能不高，需要配合cache()
//checkPoint()看成groupByKey()
rdd.toDebugString
//cache()会丢失数据，所以要保存血缘关系，而checkPoint()会持久化文件不需要保存血缘关系
```

## 自定义RDD分区器

```scala
//p103
// 1. 继承
// 2. 重写
class MyPartitioner extends Partitioner{
   //分区数量
   override def numPartitions: Int = 3
   //分区逻辑
   override def getPartition(key: Any): Int = {
     key match {
       case "nba" => 0
       case "wnba" => 1
       case _ => 2
     }
   }
}
//3. 使用
rdd.partitionBy(new MyPartitioner)
```

## RDD文件读取与保存

```scala
//write 写入
rdd.saveAsTextFile("output1/")
rdd.saveAsObjectFile("output2/")
rdd.saveAsSequenceFile("output3/")

//load 读取
val rdd1 = rdd.objectFile[(Strilng,Int)]("output2")
val rddq = rdd.sequenceFile[(Strilng,Int)]("output2")
```

## 累加器

```scala
//p105 rdd、分区、Executor的关系 重要！！！
//一个rdd包含多个分区，一个分区对应一个Task。  Executor可能有多个分区。
val rdd = sc.makeRDD(List(1,2,3,4))
val sum = 0
rdd.foreach(
	num=>{
        sum+=num
    }
)
//结果==0？？
//问题：没有分区间的计算；Executor端的sum没有返回到Driver
//解决
val sumAcc=sc.longAcculmulator("sum")
rdd.foreach(
	num=>{
        sumAcc.add(num)
    }
)
println(sumAcc.value)

//p106 累加器的多加与少加的问题，就是行动算子的多少问题。

//p107 自定义累加器 ,实现WordCount

```

<img src="E:\github\spark\7.JPG" style="zoom:80%;" />

## 广播变量

```scala
//问题 将rdd1和rdd2合并
//产生笛卡儿积，影响shuffle
val rdd1 = sc.makeRDD(List(("a",1),("b",2),("c",3)))
val rdd2 = sc.makeRDD(List(("a",4),("b",5),("c",6)))
rdd1.join(rdd2).collect()//("a",(1,4)) ("b",(2,5)) ("c",(3,6))
//解决 
//一个rdd包含多个分区，一个分区对应一个Task。  Executor可能有多个分区。
val map: Broadcast[mutable.Map[String,Int]]=sc.broadcast(map)
```

<img src="E:\github\spark\8.JPG" style="zoom:80%;" />



## 需求案例

### 转换率

单跳转化率：在一个session中，先访问3的次数a，紧接着访问5的次数b,3-5的转化率，b/a.



## ThreadLocal

```scala
val scLocal=new ThreadLocal[SparkContext]()
//sc SparkContext
scLocal.set(sc)

public void set(value){
    ThreadLocalMap map = Thread.currentThread().threadLocalMap//没有则创建
    map.set(this,value)//this==scLocal==ThreadLocal
}
```

总结：每个Thread都有不同的ThreadLocalMap，key为ThreadLocal，value为SparkContext

# SparkSQL

重点学习：DataFrame和DataSet

SparkContext转换成SparkSession

# DataFrame

```scala
//读取
val df=spark.read.json() //DataFrame
//创建临时表
df.createTempView("user")
df.createOrReplaceTempView("user")
//sql
spark.sql()

//rdd=>DataFrame
rdd.toDF("id")
```

## DataSet

```scala
case class Emp(age:Long,username:String)
//dataFrame的基础上添加类型，dataSet
val ds=df.as[Emp]
```

<img src="E:\github\spark\9.JPG" style="zoom:80%;" />





# Scala

方法既是对象，对象既是方法。可以在方法中传入一个方法参数，类似于Java传一个匿名内部类。

与go类似，可以自动推断变量类型，与js不同(弱语言类型)。



路线:不要过于纠结scala上面隐晦的语法，把握主干，`oop，集合，模式匹配，函数式编程，case 类，隐私类转化，半身对象，apply方法`，最后你有兴趣再看看scala并发编程，scala的并发相比java要简单

记住scala是jvm上c++,不要指望全面掌握，只掌握你需要，你永远不知道你是不是真的掌握了它。

```scala
//for循环
glomRDD.collect().foreach(data=>println(data.mkString(",")))

for (elem<- glomRDD.collect()){
      println(elem.mkString(","))
}
```

## 高阶函数

```scala
//函数体中有为实现的方法，通过传参的方式实现，相当于函数指针
```

## 模式匹配

```scala
//字符串，变量，_,类型
// => 表示返回的结果， case x : Int => x
//
```

## 化简原则

```scala
//参数对应&&一行 _+_
```



# 调度组件

## Oozie

主要功能：workflow:工作流，scheduler:调度

组件：workflow.xml	Coordinator.xml	Bundle.xml

# 管理组件



# 日记

做事情一定要有规划，不能随心所欲！！！

按时记录每天的学习日记！！！

定时任务的重复执行，导致数据库的重复修改。==> 定时备份数据库、观察应用的部署情况、

不断的去发现问题，解决问题，从书籍中获取思路。





一个项目的分工和时间规划到底是怎么样的安排，必须有一个切身经历的经验，否则容易南辕北辙、拔苗助长。所以说软件工程非常重要。流程的严谨，有助于开发人员的容错性。

