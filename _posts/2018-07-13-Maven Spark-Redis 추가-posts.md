---
title: "Maven Spark-Redis 추가"
categories:
  - scala
tags:
  - scala
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky: true
---

POM.xml 에 아래 내용 추가

```xml
<repositories>
    …
    <repository>
        <id>central</id>
        <name>central</name>
        <url>https://repo.maven.apache.org/maven2</url>
    </repository>
    …
</repositories>

<dependencies>
    … 
    <dependency>
        <groupId>RedisLabs</groupId>
        <artifactId>spark-redis</artifactId>
        <version>0.3.2</version>
    </dependency>
    … 
</dependencies>

<--Dependency spark streaming-->
    <dependency>
        <groupId>org.apache.spark</groupId>
        <artifactId>spark-streaming_${scala.compat.version}</artifactId>
        <version>1.6.0</version>
        <scope>provided</scope>
    </dependency>
```

```scala
//사용
import com.redislabs.provider.redis._
```

단, 디펜던시 라이브러리 추가해야함. [org.apache.spark](https://mvnrepository.com/artifact/org.apache.spark) » [spark-streaming\_2.10](https://mvnrepository.com/artifact/org.apache.spark/spark-streaming_2.10)

Spark-streaming 없으면 classpath 찾지 못해서 빌드 에러

```xml
<dependency>
    <groupId>org.apache.spark</groupId>
    <artifactId>spark-streaming_${scala.compat.version}</artifactId>
    <version>1.6.0</version>
    <scope>provided</scope>
</dependency>
```