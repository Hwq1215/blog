# SpringBoot包的管理工具Maven

> Maven官网:[https://maven.apache.org/ref/3.6.1/maven-model/maven.html](https://maven.apache.org/ref/3.6.1/maven-model/maven.html)

- 通过Maven, 我们能更便利地引用现成的开发套件
- 有利于解决版本冲突
- 在打包时，Maven会将使用的开发套件一同打包进入我们项目的jar包

# pom.xml 的编写 <!--{docsify-ignore}-->

> pom.xml 就是 maven 的配置文件,用以描述项目的各种信息

- #### 基本配置
|标签|说明|
|----|----|
|modelVersion|声明此 POM 符合哪个版本的项目描述符|
|parent|父项目的位置（如果存在）。如果未指定父项目中的值，则这些值将是此项目的默认值。该位置以组ID、项目 ID 和版本的形式给出。|
|groupId|项目的通用唯一标识符。通常使用完全限定的包名称将其与具有类似名称的其他项目区分开来|
|artifactId|此项目的标识符，在组 ID 给出的组中是唯一的。工件是由项目生成或使用的东西。Maven 为项目生成的工件示例包括：JAR、源代码和二进制发行版以及 WAR|
|version|此项目生成的工件的当前版本|
|name|项目的全名|
|description|项目的详细说明，每当需要描述项目时，例如在网站上，Maven 都会使用。虽然可以将此元素指定为 CDATA 以允许在说明中使用 HTML 标记，但不建议允许纯文本表示。如果需要修改生成的网站的索引页，则可以指定自己的索引页，而不是调整此文本|
|properties/key=value*|可在整个 POM 中用作替换的属性，如果启用，则用作资源中的筛选器的属性,格式是 `<name> value </name >`|

```xml
<modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.2.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.example</groupId>
    <artifactId>coinsBlog</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>coinsBlog</name>
    <description>coinsBlog</description>
    <properties>
        <java.version>1.8</java.version>
    </properties>
```

- #### 依赖 Dependencies
```xml
<dependencies>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.2.2</version>     <!--通过vesion标签指定版本-->
        </dependency>

    </dependencies>
```
- #### 构建 build
```xml
<build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <excludes>
                        <exclude>
                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok</artifactId>
                        </exclude>
                    </excludes>
                </configuration>
            </plugin>
        </plugins>
    </build>
```