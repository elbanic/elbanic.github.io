---
title: "Spark에서 CSV file 사용하기"
categories:
  - scala
tags:
  - scala
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky: true
---

* 스파크 옵션으로 --jars 를 사용하면 jar 파일을 디펜던시 추가.

```sh
spark-shell --jars "spark-csv_2.10-1.2.0.jar,commons-csv-1.4.jar"
```

* 스파크쉘에서 아래와 같이 사용 가능

```scala
//읽기
val dictdata = sqlContext.read.format("com.databricks.spark.csv")
    .option("header", "false")
    .load("/app/ejjeong/work/kr11st/queryanalysis.normal.brand.dic")
    .selectExpr("lower(C0) as dict")
  
//쓰기 => 저장하면 하둡과 동일하게 파일명 part-00000 으로 저장됨.
val out_path4 = "/app/ejjeong/work/kr11st/result/"
val writer = result.repartition(1).write.mode(SaveMode.Overwrite)
writer.format("com.databricks.spark.csv").option("header", "true").option("delimiter", "\t")
writer.save(out_path)
```