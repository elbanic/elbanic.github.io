---
title: "Array[Row] to DataFrame"
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
import sqlContext.implicits._
val f = e.map({
  case org.apache.spark.sql.Row(a:String,b:String,c:String,d:String,e:String,f:Int) => (a,b,c,d,e,f)
}).toDF()
```