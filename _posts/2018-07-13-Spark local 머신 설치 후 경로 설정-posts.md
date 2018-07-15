---
title: "Spark local 머신 설치 후 경로 설정"
categories:
  - scala
tags:
  - scala
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky: true
---

```sh
export PATH=$PATH:/home/1003850/temp/scala-2.11.6/bin
export SPARK_HOME=/home/1003850/temp/spark-1.6.0-bin-hadoop2.6
export PATH=$PATH:$SPARK_HOME/bin
```
> 위 내용을 bash_profile 적고
> .bash_profile 수정 후 source .bash_profile 해야 적용됨

## 원래는 아래 패키지로 실행하면 자동으로 다운을 받지만
`--packages com.databricks:spark-csv_2.11:1.2.0`

## bluegate에서 다운로드가 금지되어 있으므로 아래 jar파일을 SFTP로 옮기고 이렇게 해야함 
```scala
sqlContext.read.format("com.databricks.spark.csv").option("header", "false").load("aa.csv").show
```