---
title: maven tree使用，查看依赖关系
date: 2021-05-25 23:22:25
tags: [Maven]
---

#### maven 管理项目的依赖，可以使用如下命令查看依赖树结构

```
#打印依赖树
mvn dependency:tree 		

#将依赖树打印到文件中
mvn dependency:tree > a.txt 
```

**其他命令**

<!--more-->

```

#打印包含某些包的依赖树
mvn dependency:tree -Dincludes=com.alibaba:fastjson

#查看依赖树中包含某个artifactId的依赖链（artifactId前面加上冒号）
mvn dependency:tree -Dincludes=:fastjson

#查看依赖树中包含某个groupId的依赖链（-Dincludes后面跟上groupId）
mvn dependency:tree -Dincludes=com.alibaba

#使用verbose参数可以看冲突和重复的具体情况：
mvn dependency:tree>tree.txt -Dverbose

#查看项目依赖
mvn dependency:analyze
```



#### maven helper工具

结合idea安装对应插件。在plugins——》Marketplace 中搜索安装Maven Helper插件

安装完成之后点击对应的pom.xml就会看到有多出一个Dependency Analyzer的选项，点击就能看到对应的maven依赖的关系图，可以通过filter搜索自己想要查找的依赖。
