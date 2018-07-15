---
title: "HDFS 에서 데이터 가져오기"
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
sqlContext.read.format("com.databricks.spark.csv").option("delimiter", "\t").option("header", "true").load("hdfs:///user/1003850/data/")
```