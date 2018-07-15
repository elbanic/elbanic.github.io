---
title: "[Error] CSV : java.io.IOException: (line 1) invalid char between encapsulated token and delimiter"
categories:
  - scala
tags:
  - scala
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky: true
---

I found the solution to the problem.

One of my CSV file has an attribute as follows:

"attribute with nested "quote" "

Due to nested quote in the attribute the parser fails.

To avoid the above problem escape the nested quote as follows:

"attribute with nested """"quote"""" "

This is the one way to solve the problem.