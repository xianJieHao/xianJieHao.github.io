---
title: Spring Boot热部署：spring-boot-devtools
date: 2018-07-25 15:48:47
tags: spring
categories: spring
password:
---

spring-boot-devtools 是一个为开发者服务的一个模块，其中最重要的功能就是热部署。

当我们修改了classpath下的文件（包括类文件、属性文件、页面等）时，会重新启动应用（由于其采用的双类加载器机制，这个启动会非常快，另外也可以选择使用jrebel）。
<!--more-->
spring-boot-devtools使用了两个类加载器来实现重启（restart）机制：

base类加载器（base ClassLoader）, restart类加载器（restart ClassLoader）。

base ClassLoader：用于加载不会改变的jar（eg.第三方依赖的jar）
restart ClassLoader：用于加载我们正在开发的jar（eg.整个项目里我们自己编写的类）。当应用重启后，原先的restart ClassLoader被丢掉、重新new一个restart ClassLoader来加载这些修改过的东西，而base ClassLoader却不需要动一下。这就是devtools重启速度快的原因。
使用devtools ,只需要添加其依赖即可 :

Maven
```
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <optional>true</optional>
    </dependency>
</dependencies>
```
devtools的功能在命令行运行jar包

 java -jar XXX.jar 
或者，当应用运行在指定的 classloader的时候, 自动失效（考虑到，或许是在生产环境）。

DevTools通过检测classpath的资源文件 resources的变动来触发应用的重启。这个跟我们在 IntelliJ IDEA中, 使用Build -> Make Project，重新构建工程的效果是一样的。

默认情况下，
```
/META-INF/maven，/META-INF/resources，/resources，/static，/templates，/public
```
这些文件夹下的文件修改不会使应用重启，但是会重新加载（devtools内嵌了一个LiveReload server，当资源发生改变时，浏览器刷新）。

如果想改变默认的设置，可以自己设置不重启的目录：
```
spring.devtools.restart.exclude=static/**,public/**
```
这样的话，就只有这两个目录下的文件修改不会导致restart操作了。
如果要在保留默认设置的基础上还要添加其他的排除目录：
```
spring.devtools.restart.additional-exclude
```
如果想要使得当非classpath下的文件发生变化时应用得以重启，使用：
```
spring.devtools.restart.additional-paths
```
这样devtools就会将该目录列入了监听范围。

在application.properties文件中，关于DevTools的键值如下：
```
# ----------------------------------------
# DEVTOOLS PROPERTIES
# ----------------------------------------

# DEVTOOLS (DevToolsProperties)
spring.devtools.livereload.enabled=true # Enable a livereload.com compatible server.
spring.devtools.livereload.port=35729 # Server port.
spring.devtools.restart.additional-exclude= # Additional patterns that should be excluded from triggering a full restart.
spring.devtools.restart.additional-paths= # Additional paths to watch for changes.
spring.devtools.restart.enabled=true # Enable automatic restart.
spring.devtools.restart.exclude=META-INF/maven/**,META-INF/resources/**,resources/**,static/**,public/**,templates/**,**/*Test.class,**/*Tests.class,git.properties # Patterns that should be excluded from triggering a full restart.
spring.devtools.restart.poll-interval=1000 # Amount of time (in milliseconds) to wait between polling for classpath changes.
spring.devtools.restart.quiet-period=400 # Amount of quiet time (in milliseconds) required without any classpath changes before a restart is triggered.
spring.devtools.restart.trigger-file= # Name of a specific file that when changed will trigger the restart check. If not specified any classpath file change will trigger the restart.

# REMOTE DEVTOOLS (RemoteDevToolsProperties)
spring.devtools.remote.context-path=/.~~spring-boot!~ # Context path used to handle the remote connection.
spring.devtools.remote.debug.enabled=true # Enable remote debug support.
spring.devtools.remote.debug.local-port=8000 # Local remote debug server port.
spring.devtools.remote.proxy.host= # The host of the proxy to use to connect to the remote application.
spring.devtools.remote.proxy.port= # The port of the proxy to use to connect to the remote application.
spring.devtools.remote.restart.enabled=true # Enable remote restart.
spring.devtools.remote.secret= # A shared secret required to establish a connection (required to enable remote support).
spring.devtools.remote.secret-header-name=X-AUTH-TOKEN # HTTP header used to transfer the shared secret.
```
另外，使用Intellij的可能会遇到这个问题，即使项目使用了spring-boot-devtools，修改了类或者html、js等，idea还是不会自动重启，非要手动去make一下或者重启，就更没有使用热部署一样。出现这种情况，并不是你的配置问题，其根本原因是因为Intellij IEDA和Eclipse不同，Eclipse设置了自动编译之后，修改类它会自动编译，而IDEA在非RUN或DEBUG情况下才会自动编译（前提是你已经设置了Auto-Compile）。

