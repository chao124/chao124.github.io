---
title: Maven依赖本地jar
date: 2021-07-25 23:11:19
tags: [Maven]
---

#### Maven引入本地jar的两种方式

**1.在Maven中引入本地依赖**

你需要在`pom.xml`文件中添加一个`<dependency>`元素，并指定本地依赖的`groupId`、`artifactId`和`version`，同时使用`systemPath`属性来指定本地依赖的文件系统路径。

```xml
<dependencies>
    <dependency>
        <groupId>your.local.dependency.groupId</groupId>
        <artifactId>your-local-dependency-artifactId</artifactId>
        <version>1.0.0</version>
        <scope>system</scope>
        <systemPath>实际路径/xxx.jar</systemPath>
    </dependency>
</dependencies>
```



**2.将jar上传至本地仓库**

```
mvn install:install-file -Dfile=/path/to/your/local/dependency/jar \
                         -DgroupId=your.local.dependency.groupId \
                         -DartifactId=your-local-dependency-artifactId \
                         -Dversion=1.0.0 \
                         -Dpackaging=jar
```

