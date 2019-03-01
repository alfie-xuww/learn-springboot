## Springboot 学习

### 启动与配置

#### 知识点

1. 使用`java -java`命令启动jar包或者使用传统的war包启动方式，同时spring提供了可以执行spring scripts命令的命令行工具。

2. 系统要求，Spring Boot 2.1.3

| 工具               | 版本      |
| ---------------- | ------- |
| Maven            | 3.3 +   |
| Gradle           | 4.4 +   |
| JDK              | 8 +     |
| Spring Framework | 5.1.5 + |

3. pom文件参照以下修改，主要是`parent` 标签中的`spring-boot-starter-parent`，如果是web项目的话，则需要添加`spring-boot-starter-web` 依赖。

```properties
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.learn.springboot</groupId>
    <artifactId>learn-springboot</artifactId>
    <packaging>pom</packaging>
    <version>1.0-SNAPSHOT</version>
    <modules>
        <module>springboot-chapter1</module>
    </modules>

    <!-- Inherit defaults from Spring Boot -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.3.RELEASE</version>
    </parent>
    <!-- Add typical dependencies for a web application -->
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
    <!-- Package as an executable jar -->
    <!-- 使用spring-boot自带的maven插件，打包之后的jar包可以执行 -->
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```



#### 实例

创建一个maven项目( 也可以使用gradle进行构建 )，pom文件引入以上依赖，创建下面这个Example.java

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@EnableAutoConfiguration
public class Example {

    @RequestMapping("/")
    String home() {
        return "hello spring boot";
    }

    public static void main(String[] args) {
        SpringApplication.run(Example.class, args);
    }
}
```

启动main函数，在浏览器上输入`127.0.0.1:8080` ，获得返回结果

> hello spring boot

打包，在项目目录下输入`mvn package` 命令进行打包，如果不成功，可以先执行`mvn clean`

```vim
D:\learn-springboot>mvn package
[INFO] Scanning for projects...
[INFO]
[INFO] ---------------< com.learn.springboot:learn-springboot >----------------
[INFO] Building learn-springboot 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-resources-plugin:3.1.0:resources (default-resources) @ learn-springboot ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 0 resource
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.8.0:compile (default-compile) @ learn-springboot ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 1 source file to D:\WorkSpace\Xuww\GitHub Repository\learn-springboot\target\classes
[INFO]
[INFO] --- maven-resources-plugin:3.1.0:testResources (default-testResources) @ learn-springboot ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory D:\WorkSpace\Xuww\GitHub Repository\learn-springboot\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.8.0:testCompile (default-testCompile) @ learn-springboot ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-surefire-plugin:2.22.1:test (default-test) @ learn-springboot ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:3.1.1:jar (default-jar) @ learn-springboot ---
[INFO] Building jar: D:\WorkSpace\Xuww\GitHub Repository\learn-springboot\target\learn-springboot-1.0-SNAPSHOT.jar
[INFO]
[INFO] --- spring-boot-maven-plugin:2.1.3.RELEASE:repackage (repackage) @ learn-springboot ---
[INFO] Replacing main artifact with repackaged archive
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 3.466 s
[INFO] Finished at: 2019-03-01T16:48:13+08:00
[INFO] ------------------------------------------------------------------------
```

执行 `jar tvf jar包名称`，可以查看jar包中详细目录和依赖的jar包，可以参考jar命令用法。

```
D:\learn-springboot\target>jar tvf learn-springboot-1.0-SNAPSHOT.jar

     0 Fri Mar 01 16:48:12 CST 2019 META-INF/
   538 Fri Mar 01 16:48:12 CST 2019 META-INF/MANIFEST.MF
     0 Fri Mar 01 16:48:12 CST 2019 org/
     0 Fri Mar 01 16:48:12 CST 2019 org/springframework/
     0 Fri Mar 01 16:48:12 CST 2019 org/springframework/boot/
     0 Fri Mar 01 16:48:12 CST 2019 org/springframework/boot/loader/
     0 Fri Mar 01 16:48:12 CST 2019 org/springframework/boot/loader/data/
  2688 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/data/RandomAccessDataFile$DataInputStream.class
     0 Fri Mar 01 16:48:12 CST 2019 org/springframework/boot/loader/jar/
  5267 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/CentralDirectoryFileHeader.class
  3263 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/data/RandomAccessDataFile$FileAccess.class
     0 Fri Mar 01 16:48:12 CST 2019 org/springframework/boot/loader/archive/
  1487 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/archive/ExplodedArchive$FileEntryIterator$EntryComparator.class
  3837 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/archive/ExplodedArchive$FileEntryIterator.class
   282 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/data/RandomAccessDataFile$1.class
  3116 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/CentralDirectoryEndRecord.class
 11548 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/Handler.class
  5243 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/archive/ExplodedArchive.class
  4015 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/data/RandomAccessDataFile.class
  4624 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/CentralDirectoryParser.class
  1813 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/ZipInflaterInputStream.class
   302 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/archive/Archive$Entry.class
   273 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/archive/ExplodedArchive$1.class
   485 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/data/RandomAccessData.class
   437 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/archive/Archive$EntryFilter.class
  7336 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/archive/JarFileArchive.class
  1953 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/PropertiesLauncher$PrefixMatchingArchiveFilter.class
  1484 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/PropertiesLauncher$ArchiveEntryFilter.class
   266 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/PropertiesLauncher$1.class
 19737 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/PropertiesLauncher.class
  4684 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/Launcher.class
  1502 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/MainMethodRunner.class
  3608 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/ExecutableArchiveLauncher.class
  1721 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/WarLauncher.class
  1585 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/JarLauncher.class
     0 Fri Mar 01 16:48:12 CST 2019 org/springframework/boot/loader/util/
  5203 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/util/SystemPropertyUtils.class
  1535 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/LaunchedURLClassLoader$UseFastConnectionExceptionsEnumeration.class
  5699 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/LaunchedURLClassLoader.class
   616 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/Bytes.class
   702 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/JarURLConnection$1.class
  1779 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/archive/JarFileArchive$EntryIterator.class
  4306 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/JarURLConnection$JarEntryName.class
  1081 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/archive/JarFileArchive$JarFileEntry.class
  9854 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/JarURLConnection.class
   945 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/archive/Archive.class
  1233 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/JarFile$2.class
  2062 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/JarFile$1.class
  1374 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/JarFile$JarFileType.class
 15076 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/JarFile.class
  4976 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/AsciiBytes.class
  1593 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/JarFileEntries$1.class
  2046 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/JarFileEntries$EntryIterator.class
   540 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/CentralDirectoryVisitor.class
   299 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/JarEntryFilter.class
  3619 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/JarEntry.class
   345 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/FileHeader.class
  3650 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/StringSequence.class
 14087 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/jar/JarFileEntries.class
  1102 Fri Feb 15 10:26:10 CST 2019 org/springframework/boot/loader/archive/ExplodedArchive$FileEntry.class
     0 Fri Mar 01 16:48:12 CST 2019 BOOT-INF/
     0 Fri Mar 01 16:48:12 CST 2019 BOOT-INF/classes/
     0 Fri Mar 01 16:48:14 CST 2019 BOOT-INF/classes/com/
     0 Fri Mar 01 16:48:14 CST 2019 BOOT-INF/classes/com/learn/
     0 Fri Mar 01 16:48:14 CST 2019 BOOT-INF/classes/com/learn/springboot/
     0 Fri Mar 01 16:48:14 CST 2019 META-INF/maven/
     0 Fri Mar 01 16:48:14 CST 2019 META-INF/maven/com.learn.springboot/
     0 Fri Mar 01 16:48:14 CST 2019 META-INF/maven/com.learn.springboot/learn-springboot/
   985 Fri Mar 01 16:48:14 CST 2019 BOOT-INF/classes/com/learn/springboot/Example.class
  1285 Fri Mar 01 16:44:46 CST 2019 META-INF/maven/com.learn.springboot/learn-springboot/pom.xml
   113 Fri Mar 01 16:48:14 CST 2019 META-INF/maven/com.learn.springboot/learn-springboot/pom.properties
     0 Fri Mar 01 16:48:12 CST 2019 BOOT-INF/lib/
   406 Fri Feb 15 10:41:44 CST 2019 BOOT-INF/lib/spring-boot-starter-web-2.1.3.RELEASE.jar
   398 Fri Feb 15 10:41:22 CST 2019 BOOT-INF/lib/spring-boot-starter-2.1.3.RELEASE.jar
948706 Fri Feb 15 10:08:36 CST 2019 BOOT-INF/lib/spring-boot-2.1.3.RELEASE.jar
1257845 Fri Feb 15 10:18:16 CST 2019 BOOT-INF/lib/spring-boot-autoconfigure-2.1.3.RELEASE.jar
   407 Fri Feb 15 10:41:22 CST 2019 BOOT-INF/lib/spring-boot-starter-logging-2.1.3.RELEASE.jar
290339 Fri Mar 31 21:27:54 CST 2017 BOOT-INF/lib/logback-classic-1.2.3.jar
471901 Fri Mar 31 21:27:16 CST 2017 BOOT-INF/lib/logback-core-1.2.3.jar
 41203 Thu Mar 16 17:36:32 CST 2017 BOOT-INF/lib/slf4j-api-1.7.25.jar
 17522 Tue Feb 05 18:14:24 CST 2019 BOOT-INF/lib/log4j-to-slf4j-2.11.2.jar
266283 Tue Feb 05 18:11:30 CST 2019 BOOT-INF/lib/log4j-api-2.11.2.jar
  4596 Thu Mar 16 17:37:48 CST 2017 BOOT-INF/lib/jul-to-slf4j-1.7.25.jar
 26586 Wed Feb 21 15:54:16 CST 2018 BOOT-INF/lib/javax.annotation-api-1.3.2.jar
1293377 Wed Feb 13 05:32:02 CST 2019 BOOT-INF/lib/spring-core-5.1.5.RELEASE.jar
 23725 Wed Feb 13 05:31:54 CST 2019 BOOT-INF/lib/spring-jcl-5.1.5.RELEASE.jar
301298 Mon Aug 27 16:23:36 CST 2018 BOOT-INF/lib/snakeyaml-1.23.jar
   405 Fri Feb 15 10:41:44 CST 2019 BOOT-INF/lib/spring-boot-starter-json-2.1.3.RELEASE.jar
1347236 Sat Dec 15 21:59:06 CST 2018 BOOT-INF/lib/jackson-databind-2.9.8.jar
 66519 Sat Jul 29 20:53:26 CST 2017 BOOT-INF/lib/jackson-annotations-2.9.0.jar
325619 Sat Dec 15 13:19:12 CST 2018 BOOT-INF/lib/jackson-core-2.9.8.jar
 33391 Sat Dec 15 23:06:14 CST 2018 BOOT-INF/lib/jackson-datatype-jdk8-2.9.8.jar
100674 Sat Dec 15 23:06:24 CST 2018 BOOT-INF/lib/jackson-datatype-jsr310-2.9.8.jar
  8642 Sat Dec 15 23:06:04 CST 2018 BOOT-INF/lib/jackson-module-parameter-names-2.9.8.jar
   406 Fri Feb 15 10:41:44 CST 2019 BOOT-INF/lib/spring-boot-starter-tomcat-2.1.3.RELEASE.jar
3287907 Mon Feb 04 16:31:00 CST 2019 BOOT-INF/lib/tomcat-embed-core-9.0.16.jar
250080 Mon Feb 04 16:31:02 CST 2019 BOOT-INF/lib/tomcat-embed-el-9.0.16.jar
265364 Mon Feb 04 16:31:02 CST 2019 BOOT-INF/lib/tomcat-embed-websocket-9.0.16.jar
1155701 Fri Jan 04 14:52:48 CST 2019 BOOT-INF/lib/hibernate-validator-6.0.14.Final.jar
 93107 Tue Dec 19 16:23:28 CST 2017 BOOT-INF/lib/validation-api-2.0.1.Final.jar
 66469 Wed Feb 14 13:23:28 CST 2018 BOOT-INF/lib/jboss-logging-3.3.2.Final.jar
 66540 Tue Mar 27 18:35:34 CST 2018 BOOT-INF/lib/classmate-1.4.0.jar
1381453 Wed Feb 13 05:32:56 CST 2019 BOOT-INF/lib/spring-web-5.1.5.RELEASE.jar
672558 Wed Feb 13 05:32:08 CST 2019 BOOT-INF/lib/spring-beans-5.1.5.RELEASE.jar
800464 Wed Feb 13 05:33:32 CST 2019 BOOT-INF/lib/spring-webmvc-5.1.5.RELEASE.jar
368947 Wed Feb 13 05:32:22 CST 2019 BOOT-INF/lib/spring-aop-5.1.5.RELEASE.jar
1099682 Wed Feb 13 05:32:30 CST 2019 BOOT-INF/lib/spring-context-5.1.5.RELEASE.jar
280409 Wed Feb 13 05:32:24 CST 2019 BOOT-INF/lib/spring-expression-5.1.5.RELEASE.jar
```

执行`java -jar learn-springboot-1.0-SNAPSHOT.jar`，在浏览器上输入`http://127.0.0.1:8080/`，可以看到一下效果

> hello spring boot

#### 注解

@RestController 告诉spring，把结果直接以字符串形式返回给调用者。

@RequestMapping 提供路由信息。

@EnableAutoConfigration 加上该注解，springboot会基于依赖的jar包进行自动配置，如果依赖了`spring-boot-starter-web` 这个jar包，spring boot就会以web项目的配置进行启动。

> 自动配置注解是为了配合`starters` 设计的，但是两者之间没有必要的联系，因为在starter依赖之外，仍然可以配置其他的jar包依赖，spring boot的自动配置会根据这些依赖决定使用哪种配置。
