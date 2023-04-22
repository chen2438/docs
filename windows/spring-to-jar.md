---
description: 发布于 2023-04-22
categories:
- windows
date: 2023-04-22
slug: spring-to-jar
title: SpringBoot将项目打包成jar包
---

# SpringBoot 将项目打包成 jar 包

参考：https://www.jianshu.com/p/84883627db67

#### 1.在pom.xml文件中导入Springboot的maven依赖（有的项目自带此代码）

```xml
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```



![image-20230422120221835](https://media.opennet.top/i/2023/04/22/64435c4f4fb25.png)

![image-20230422120309756](https://media.opennet.top/i/2023/04/22/64435c7e8cfca.png)

#### 2.package一下

![image-20230422120528919](https://media.opennet.top/i/2023/04/22/64435d0972886.png)

![image-20230422120605640](https://media.opennet.top/i/2023/04/22/64435d2e42c4d.png)

双击package，稍等一会

![image-20230422120643639](https://media.opennet.top/i/2023/04/22/64435d5477653.png)

如图所示为构建成功

#### 3.package完成以后,target中会生成一个.jar包

![image-20230422120758447](https://media.opennet.top/i/2023/04/22/64435d9f18f7f.png)

![image-20230422120926580](https://media.opennet.top/i/2023/04/22/64435df77a16c.png)

在终端中打开此目录

然后`java -jar demo-0.0.1-SNAPSHOT.jar --server.port=8086` 即可运行jar包

server.port用于设置端口

![image-20230422121152992](https://media.opennet.top/i/2023/04/22/64435e8a2d815.png)

```shell
PS E:\Projects\Github\java\demo\target> java -jar demo-0.0.1-SNAPSHOT.jar --server.port=8086

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::               (v2.7.11)

2023-04-22 12:09:58.924  INFO 49844 --- [           main] com.example.demo.DemoApplication         : Starting DemoApplication v0.0.1-SNAPSHOT using Java 1.8.0_291 on Legion with PID 49844 (E:\Projects\Github\java\demo\target\demo-0.0.1-SNAPSHOT.jar started by Qwer-laptop in E:\Projects\Github\java\demo\target)
2023-04-22 12:09:58.927  INFO 49844 --- [           main] com.example.demo.DemoApplication         : No active profile set, falling back to 1 default profile: "default"
2023-04-22 12:09:59.970  INFO 49844 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 8086 (http)
2023-04-22 12:09:59.980  INFO 49844 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2023-04-22 12:09:59.980  INFO 49844 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.74]
2023-04-22 12:10:00.107  INFO 49844 --- [           main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2023-04-22 12:10:00.107  INFO 49844 --- [           main] w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 1135 ms
2023-04-22 12:10:00.416  INFO 49844 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8086 (http) with context path ''
2023-04-22 12:10:00.423  INFO 49844 --- [           main] com.example.demo.DemoApplication         : Started DemoApplication in 1.813 seconds (JVM running for 2.2)
2023-04-22 12:10:12.119  INFO 49844 --- [nio-8086-exec-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring DispatcherServlet 'dispatcherServlet'
2023-04-22 12:10:12.119  INFO 49844 --- [nio-8086-exec-1] o.s.web.servlet.DispatcherServlet        : Initializing Servlet 'dispatcherServlet'
2023-04-22 12:10:12.120  INFO 49844 --- [nio-8086-exec-1] o.s.web.servlet.DispatcherServlet        : Completed initialization in 1 ms
```

