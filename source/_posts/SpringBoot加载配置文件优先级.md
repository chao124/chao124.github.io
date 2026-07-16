---
title: SpringBoot加载配置文件优先级
date: 2022-10-07 20:56:22
tags: [springboot]
---

当项目启动时，Spring Boot会按照一定的顺序去加载这些配置文件。以下是这些配置文件的默认加载顺序：

#### 读取顺序
优先级由高到底，高优先级的配置会覆盖低优先级的配置。
- 如果低优先级存在高优先级没有的属性，则会互补配置。
- 如果同一个配置属性，在多个配置文件都配置了，默认使用第1个读取到的即优先级高的配置。
- 在同一位置下，`.properties` 文件优先级高于 `.yml` 或 `.yaml` 文件，且带有具体环境标识（如开发、生产环境）的配置文件优先于通用配置文件。
<!--more-->
##### 1.命令行参数：

通过 `--spring.config.location` 或 `--spring.config.additional-location` 指定的配置文件路径拥有最高的优先级。

**另外：**
spring.config.location：这个会使项目原本的配置文件失效。
spring.config.additional-location：不会使项目内置的配置文件失效，两者会互补，additional-location配置的文件优先级更高。

##### 2.Java系统属性：

通过`System.setProperty()`设置的属性。

##### 3.操作系统环境变量：

所有以 SPRING_APPLICATION_JSON 形式存在的环境变量，以及以 spring.* 开头的环境变量会被转换为配置属性。

##### 4.配置文件外部：

`config/{name}-{profile}.properties 或 config/{name}-{profile}.yml`
`{name}-{profile}.properties 或 {name}-{profile}.yml`
`config/{name}.properties 或 config/{name}.yml`
`{name}.properties 或 {name}.yml`

其中 {name} 默认为 application，{profile} 是激活的Spring Boot配置环境。
可通过命令行激活

```
java -jar myapp.jar --spring.profiles.active=prod
```

##### 5.配置文件内部（对于已打包的应用）：

`BOOT-INF/classes/config/{name}-{profile}.properties 或 BOOT-INF/classes/config/{name}-{profile}.yml`
`BOOT-INF/classes/{name}-{profile}.properties 或 BOOT-INF/classes/{name}-{profile}.yml`
`BOOT-INF/classes/config/{name}.properties 或 BOOT-INF/classes/config/{name}.yml`
`BOOT-INF/classes/{name}.properties 或 BOOT-INF/classes/{name}.yml`

##### 6.`@Configuration` 类上的 `@PropertySource` 注解：

这种方式加载的配置具有较低的优先级，但是可以直接指向具体的配置文件。

##### 7.默认配置

Spring Boot包含一些内建的默认配置，其优先级最低。

