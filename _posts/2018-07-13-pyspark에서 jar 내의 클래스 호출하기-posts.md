---
title: "pyspark에서 jar 내의 클래스 호출하기"
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
# 시작할 때 절대 경로로 jar 들고 들어가기, class-path 지정
pyspark \
--jars /app/irteam/ejjeong/temp/nlp_indexterm-1.5.6-jar-with-dependencies.jar \
--driver-class-path /app/irteam/ejjeong/temp/nlp_indexterm-1.5.6-jar-with-dependencies.jar
```

```python
# python interpreter에서 sc._jvm.패키지명
sc._jvm.com.skplanet.nlp.indexingterms.IndexingTermsAPI("a",1)
```