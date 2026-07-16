---
title: Maven中的dependencyManagement作用
date: 2021-08-18 12:22:14
tags: [Maven]
---

#### 1.作用:

在Maven中dependencyManagement的作用其实相当于一个对所依赖jar包进行版本管理的管理器。

<!--more-->

#### 2.pom.xml文件中，jar的版本判断的两种途径:

(1)如果dependencies里的dependency自己没有声明version元素，那么maven就会到dependencyManagement里面去找有没有对该artifactId和groupId进行过版本声明，如果有，就继承它，如果没有就会报错，告诉你必须为dependency声明一个version。

(2)如果dependencies中的dependency声明了version，那么无论dependencyManagement中有无对该jar的version声明，都以dependency里的version为准。

#### 3.dependencyManagement与dependencies区别:

(1)dependencies 即使在子项目中不写该依赖项，那么子项目仍然会从父项目中继承该依赖项（全部继承）

注意：这里的dependencies 并不是dependencyManagement下面的`<dependencies></dependencies>` , 而是外部的`<dependencies></dependencies>`

(2)dependencyManagement里只是声明依赖，并不实现引入。

(3)因此子项目需要显示的声明需要用的依赖。如果不在子项目中声明依赖，是不会从父项目中继承下来的；只有在子项目中写了该依赖项，并且没有指定具体版本，才会从父项目中继承该项，并且version和scope都读取自父pom;另外如果子项目中指定了版本号，那么会使用子项目中指定的jar版本。

**特别需要注意的是，`DependencyManagement`标签下声明引用的依赖并不会真正的下载引入到maven仓库中，相当于一个模板声明，类似于接口的定义，只做方法的定义，而不做实现，实现交给实现接口的类去完成。这里就会交给子模块去完成，只有当子模块`Dependencies`标签下做了真正的依赖引入，才会下载引入相关依赖。**

##### 示例

父工程pom.xml文件相关配置：

```
<dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>Finchley.SR2</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>fastjson</artifactId>
                <version>1.2.61</version>
            </dependency>
        </dependencies>
</dependencyManagement>
```

子模块相关配置：

```
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-oauth2</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>

        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.72</version>
        </dependency>
```

**如上示例所示，org.springframework.cloud没有重新声明版本号，使用的是父工程的引入版本声明，而com.alibaba重新声明了版本号，所以调用的是子模块中的声明。**

#### 4.使用DependencyManagement的好处

这样做的好处是，统一了项目依赖的版本号，确保各个子模块与父工程的依赖和版本一致，当你需要修改相关依赖的版本号时，你只需要修改父工程中的版本号，子模块引入的依赖和版本号将会同时被修改，大大提高了项目的管理性能。当某个子模块需要不同版本依赖时，你也可以显示声明相关依赖版本号，覆盖父工程中的版本依赖声明。
