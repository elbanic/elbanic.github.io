---
title: "Redis의 특성상 데이터가 큰 패킷은 유용하지 않다. (작은 데이터 Key-Value Pair에 유용)"
categories:
  - scala
tags:
  - scala
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky: true
---

```scala
def writeDFtoRedis(tbname: String, df: DataFrame): Unit ={

  val rdd = df.rdd.map(r => r.mkString("\t"))
  val schema = df.schema.toArray

  //indexWhere로 컬럼 순서를 맞춘다
  val schema_arr = schema.map(r => Array(schema.indexWhere(_==r), r.name, r.dataType, r.nullable).mkString("\t"))
  val schema_rdd = sc.parallelize(schema_arr.toSeq)

  val tableNM = FilePath.getCurDir + "_" + tbname + "_table"
  val schemaNM = FilePath.getCurDir + "_" + tbname + "_schema"

  sc.toRedisSET(rdd, tableNM)
  sc.toRedisSET(schema_rdd, schemaNM)
  DPLogger.logging(s"Push DataFrame to Redis : $tableNM")
}

def loadDFfromRedis(sqlContext: SQLContext, tbname: String): DataFrame ={

  val tableNM = FilePath.getCurDir + "_" + tbname + "_table"
  val schemaNM = FilePath.getCurDir + "_" + tbname + "_schema"

  val keysRDD = sc.fromRedisSet(Array(tableNM), availthreads)
  val schemaRDD = sc.fromRedisSet(Array(schemaNM))

  val listRDD = keysRDD
  //r.split("\t")(0)은 컬럼 순서
  val slistRDD = schemaRDD.sortBy(r => r.split("\t")(0).asInstanceOf[String].toInt)

  val schema = StructType(slistRDD.map(r => StructField(r.split("\t")(1), StringType, true)).collect)
  val rdd = listRDD.map(r => Row(r.split("\t").toSeq:_*))
  sqlContext.createDataFrame(rdd, schema).distinct
}
```

## Ignite의 경우 GC를 많이 사용하게 된다…

> 설정값을 잘 건들여봐야겠다

```scala
import org.apache.ignite.spark.IgniteContext
import org.apache.ignite.configuration._
import org.apache.spark.{SparkConf, SparkContext}
 
object RDDProducer extends App {
  val conf = new SparkConf().setAppName("SparkIgnite")
  val sc = new SparkContext(conf)
  val ic = new IgniteContext[Int, Int](sc, () => new IgniteConfiguration())
  val sharedRDD: IgniteRDD[Int,Int] = ic.fromCache("partitioned")
  sharedRDD.savePairs(sc.parallelize(1 to 100000, 10).map(i => (i, i)))
}
 
object RDDConsumer extends App {
  val conf = new SparkConf().setAppName("SparkIgnite")
  val sc = new SparkContext(conf)
  val ic = new IgniteContext[Int, Int](sc, () => new IgniteConfiguration())
  val sharedRDD = ic.fromCache("partitioned")
  val lessThanTen = sharedRDD.filter(_._2 < 10)
  println("The count is:::::::::::: "+lessThanTen.count())
}

val ic = new IgniteContext[String, Row](sc, "/app/irteam/ejjeong/apache-ignite-fabric-1.6.0-bin/config/default-config.xml")

def writeDFtoIgnite(ic: IgniteContext[String, Row], tbname: String, df: DataFrame): Unit ={

  val rdd = df.rdd.map(r => (r.getAs[String](0), r))
  val schema = df.schema.toArray

  //indexWhere로 컬럼 순서를 맞춘다
  val schema_arr = schema.map(r => Array(schema.indexWhere(_==r), r.name, r.dataType, r.nullable).mkString("\t"))
  val schema_rdd = sc.parallelize(schema_arr.map(r => (r.asInstanceOf[String], Row(r))))

  val tableNM = "cacheTest" + "_" + tbname + "_table"
  val schemaNM = "cacheTest" + "_" + tbname + "_schema"

  val sharedRdd = ic.fromCache(tableNM)
  sharedRdd.savePairs(rdd)
//  sharedRdd.savePairs(schema_rdd)
}

def loadDFfromIgnite(ic: IgniteContext[String, Row], tbname: String): DataFrame ={

  val tableNM = "cacheTest" + "_" + tbname + "_table"
  val schemaNM = "cacheTest" + "_" + tbname + "_schema"

  val sharedRdd = ic.fromCache(tableNM)
  sqlContext.createDataFrame(sharedRdd)
}
```

> 파일이 가장 좋긴 하네..?
> 그럼 하둡을 설치하는 건 어떨까? HDFS에 저장해서 Hive로 조회도 쉬울것 같은데.

![](resources/A0AA9E1D8E21B3EF6DEE4B6D2CCBC7C0.jpg)