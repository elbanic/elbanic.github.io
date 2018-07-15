---
title: "ignite 설치"
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
wget apache-ignite-fabric-2.0.0-b1-bin.zip

unzip apache-ignite-fabric-2.0.0-b1-bin.zip
cd apache-ignite-fabric-2.0.0-b1-bin

export IGNITE_HOME=<path-to-ignite-installation-folder>

bin/ignite.sh examples/config/example-ignite.xml
```