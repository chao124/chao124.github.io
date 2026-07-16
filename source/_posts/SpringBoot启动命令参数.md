---
title: SpringBoot启动命令参数
date: 2022-09-23 11:09:23
tags: [springboot]
---

##### SpringBoot启动参数

基于SpringBoot开发的项目，直接打包成jar文件，基于内嵌的tomcat启动命令。

```
java [ options ] -jar *.jar [ arguments ]
```

<!--more-->

##### 常见配置

```
--server.port：指定应用程序的端口号
--spring.profiles.active：设置应用程序使用的配置文件中的环境配置
--spring.config.additional-location：指定额外的配置文件路径
--Xms：设置JVM初始堆大小
--Xmx：设置JVM最大堆大小
--XX:PermSize：设置JVM永久代大小
--XX:MaxPermSize：设置JVM最大永久代大小
--Xdebug：开启远程JDWP调试
-D：定义属性
```

##### options 【-D】虚拟机参数

**【-D】要放到 -jar 前面，否则参数无效**

在启动参数中，我们可以通过添加这样的配置，来覆盖系统属性中的值：

``` 
java -Dfile.encoding=UTF-8 -jar app.jar 
```

在代码中可以通过这样获取该值：

```java
String fileEncoding = System.getProperties("file.encoding"); //UTF-8
```

##### arguments 【--】命令行参数

**【--】参数不能放到jar包前面，否则会报错**

```
java -Dfile.encoding=UTF-8 -jar app.jar --server.port=8080 
```

可以在main方法的参数中获取该值:

```java
log.info(">>>>> args: {}", Arrays.toString(args) );
```

或者：

代码中是通过`main`函数参数 `String[] args` 传入再通过`SpringApplication.run(App.class, args)`传入`springboot`进行解析的可以通过实现 `EnvironmentAware接口` 注入环境对象，可以读取命令行参数

```java
@SpringBootApplication
public class App implements EnvironmentAware {
 
    static Environment environment;
 
    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
 
        // 1234
        System.out.println(System.getProperty("server.port"));
        // 8080  同名的命令行参数覆盖虚拟机参数
        System.out.println(environment.getProperty("server.port"));
 
        System.out.println(environment.getProperty("user.dir"));
 
        System.out.println("*****启动成功*****");
    }
    // 注入环境对象
    @Override
    public void setEnvironment(Environment environment) {
        App.environment = environment;
    }
}
```

###### 
