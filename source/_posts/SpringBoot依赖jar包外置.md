---
title: SpringBoot依赖jar包外置
date: 2023-06-18 10:55:19
tags: [springboot,Maven]
---

##### 背景

SpringBoot编译出来的jar所有的依赖都在jar包内部，导致jar包比较大，网络慢的话每次部署上传都要很长时间。

##### 解决方法

###### 修改`pom.xml`文件

<!--more-->

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <!-- 指定该jar包启动时的主类[建议] -->
                <mainClass>
                    com.sc.springdata.SpringdataApplication
                </mainClass>
                <!--使用-Dloader.path需要在打包的时候增加<layout>ZIP</layout>，不指定的话-Dloader.path不生效-->
                <layout>ZIP</layout>

                <includes>
                    <!--这里是填写需要包含进去的jar，项目中必须的某些模块，会经常变动，那么就应该将其坐标写进来，如果没有则 null ，表示不打包依赖 -->
                    <include>
                        <groupId>null</groupId>
                        <artifactId>null</artifactId>
                    </include>
<!--						<include>-->
<!--                            <groupId>org.projectlombok</groupId>-->
<!--                            <artifactId>lombok</artifactId>-->
<!--						</include>-->
<!--                        <dependency>-->
<!--							<groupId>org.projectlombok</groupId>-->
<!--							<artifactId>lombok</artifactId>-->
<!--						</dependency>-->
                </includes>
            </configuration>
            <executions>
                <execution>
                    <goals>
                        <goal>repackage</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
         <!-- 拷贝项目所有依赖jar文件到构建lib目录下 -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-dependency-plugin</artifactId>
            <executions>
                <execution>
                    <id>copy-dependencies</id>
                    <phase>package</phase>
                    <goals>
                        <goal>copy-dependencies</goal>
                    </goals>
                    <configuration>
                        <!--
                        各子模块按照实际层级定义各模块对应的属性值，检查所有微服务模块依赖jar文件合并复制到同一个目录
                        详见各子模块中 boot-jar-output 属性定义
                        -->
<!--                        <includeTypes>jar</includeTypes>-->
                        <!--这里表示只打包scope为runtime 的包-->
<!--                        <includeScope>runtime</includeScope>-->
                        <!-- ${project.build.directory}是maven变量，内置的，表示target目录,如果不写，将在跟目录下创建/libs -->
                        <outputDirectory>libs</outputDirectory>
                        <!--excludeTransitive:是否不包含间接依赖包，比如我们依赖A，但是A又依赖了B，我们是否也要把B打进去 默认不打-->
                        <excludeTransitive>false</excludeTransitive>
                        <!--复制的jar文件去掉版本信息-->
                        <stripVersion>false</stripVersion>
                        <silent>false</silent>
                    </configuration>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

###### 修改启动命令

这种方式打的包，在项目启动时，需要通过 -Dloader.path 指定 lib 的路径

`java -Dloader.path=./lib -jar xxx.jar`

使用这种方式部署，虽然打的包小了，不用每次都上传一个很大的 jar 包，从而节省部署时间。但这种方式也有一个弊端就是增加了Jar包的管理成本，多人协调开发，构建的时候，还需要专门去关注是否有人更新依赖。

##### maven 的内置变量

```
1. ${basedir} 项目根目录
2. ${project.build.directory} 构建目录，缺省为target
3. ${project.build.outputDirectory} 构建过程输出目录，缺省为target/classes
4. ${project.build.finalName} 产出物名称，缺省为${project.artifactId}-${project.version}
5. ${project.packaging} 打包类型，缺省为jar
6. ${project.xxx} 当前pom文件的任意节点的内容 
```

