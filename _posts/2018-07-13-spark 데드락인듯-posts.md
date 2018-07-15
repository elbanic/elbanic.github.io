---
title: "spark 데드락?"
categories:
  - scala
tags:
  - scala
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky: true
---

```bash
//img 매핑
val man_prd1_for_img = man_not_matched.selectExpr("prd_no as prd_no1", "prd_nm as prd_nm1",
  "feature_prd as feature", "prd_model_sortedText as prd_model_sortedText1")
  .filter("feature is not null")
val woman_prd1_for_img = woman_not_matched.selectExpr("prd_no as prd_no1", "prd_nm as prd_nm1",
  "feature_prd as feature", "prd_model_sortedText as prd_model_sortedText1")
  .filter("feature is not null")
val man_arr_buf = ArrayBuffer(man_prd1_for_img.collect.toList.toSeq:_*).sortBy(r=>r(2).asInstanceOf[String])
val woman_arr_buf = ArrayBuffer(woman_prd1_for_img.collect.toList.toSeq:_*).sortBy(r=>r(2).asInstanceOf[String])

//model 매핑
val man_prd1_for_mdl = man_not_matched.selectExpr("prd_no as prd_no1", "prd_nm as prd_nm1",
  "prd_model_sortedText", "feature_prd as feature1")
val woman_prd1_for_mdl = woman_not_matched.selectExpr("prd_no as prd_no1", "prd_nm as prd_nm1",
  "prd_model_sortedText", "feature_prd as feature1")
val man_model_arr_buf = ArrayBuffer(man_prd1_for_mdl.filter("prd_model_sortedText!=''").collect.toList.toSeq:_*).sortBy(r=>r(2).asInstanceOf[String])
val woman_model_arr_buf = ArrayBuffer(woman_prd1_for_mdl.filter("prd_model_sortedText!=''").collect.toList.toSeq:_*).sortBy(r=>r(2).asInstanceOf[String])
```

스팍 쉘에서 위 코드를 돌리는데, paste로 돌리면 4개의 collect 함수가 돌면서 갑자기 데드락이 걸린 듯한 현상이 보인다…

원인이 뭘까?