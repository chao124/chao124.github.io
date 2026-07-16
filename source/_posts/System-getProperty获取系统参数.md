---
title: System.getProperty获取系统参数
date: 2022-05-11 13:57:57
tags: [springboot]
---

对于Java平台来说，它使用一个Properties对象来维护自己的配置信息。System类维护了一个Properties对象，这个对象描述了当前工作环境的配置信息系统配置信息包括了当前的用户、当前的java版本、文件分隔符等等。

<!--more-->

| key             | 含义                                        |
| :-------------- | ------------------------------------------- |
| file.separator  | 文件分隔符                                  |
| java.class.path | 获取所有classpath路径，获取到的是一个字符串 |
| java.home       | Java安装目录                                |
| java.vendor     | Java运行时环境供应商                        |
| java.vendor.url | Java供应商的URL                             |
| path.separator  | 路径分隔符（在 UNIX 系统中是“:”）           |
| line.separator  | 行分隔符（在 UNIX 系统中是“/n”）            |
| os.arch         | 操作系统的架构                              |
| os.name         | 操作系统的名称                              |
| os.version      | 操作系统的版本                              |
| user.dir        | 当前用户的工作目录                          |
| user.home       | 当前用户的主目录                            |
| user.name       | 当前用户的用户名称                          |

```java
public static void main(String[] args) throws Exception {
       //使用System.getProperty()来获取系统参数
       //常见的系统参数获取：
       //根据指定键获取相应的系统参数
       // 用户当前的工作目录      key 为： user.dir
       // 用户的账户名称          key 为： user.name
       // 用户的主目录            key 为： user.home
       // 文件分隔符              key 为： file.separator
       String property = System.getProperty("user.dir");
       System.out.Println(property);
   }
```

