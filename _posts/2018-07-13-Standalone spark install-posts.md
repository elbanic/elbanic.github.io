---
title: "Standalone spark install"
categories:
  - scala
tags:
  - scala
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky: true
---

# 아래 두 파일 설정 후

```sh
conf/spark-env.sh
conf/spark-defaults.conf
```

# 마스터 실행 - 워커 실행

```sh
sbin/start-master.sh
sbin/start-slave.sh spark://your\_host\_name:7077 -m 512M -c 10
```