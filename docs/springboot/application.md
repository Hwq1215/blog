# 配置文件
> SpringBoot可以将将需要配置的变量抽取到一个统一的文件中，这个文件被称为配置文件，默认为application.properties，application.yml，application.yaml三种（优先级由高到低排序）。并且yml和yaml是一样的语法。

# application.properties
> 键值对，通过等号连接,语法相对不严格，但是书写繁杂

```properties
#端口暴露
server.port = 8080

#数据库连接配置
spring.datasource.url = jdbc:mysql://localhost:3306/data?useUnicode=true&useSSL=false&characterEncoding=utf8&autoReconnect=true&failOverReadOnly=false
spring.datasource.username = root
spring.datasource.password = mypassword
spring.datasource.driver-class-name = com.mysql.cj.jdbc.Driver
```

# application.yml | application.yaml
> 通过冒号连接，相同父级可以写在一起，语法严格，要特别注意:后有空格，比较简洁明了。
```yaml
#端口暴露
server:
  port: 8081

#数据库连接配置
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/data?useUnicode=true&useSSL=false&characterEncoding=utf8&autoReconnect=true&failOverReadOnly=false
    username: root
    password: mypassword
    driver-class-name: com.mysql.cj.jdbc.Driver

```

# 将自定义的值从配置文件取出

> 我们自定义的值
```yaml
    person:
        name: user
        job: cook
```

1. 直接通过注解@Value取出

- 先写一个类
```java
    @Component  //声明为组件
    public class JobComponent{
        @Value("${person.cook}")
        private String job;

        public String getJob(){
            return job;
        }
    }
```
- 在用这个类时要用手动@Autowired,在一个Controller类中调用
```java
    @RestController
    public class JobController{
        @Autowired
        private JobComponent myJobComponent = new JobComponent();

        System.out.println(myJobComponent.getJob());
    }
```

2. 通过@ConfigurationProperties封装一组数据
- 同样先声明一个类
```java
     @Component  //声明为组件
     @ConfigurationProperties("person")
    public class JobComponent{
        private String name;
        private String job;

        public String getName(){
            return job;
        }

        public String getName(){
            return job;
        }
    }
```
- 也要用@Autowired注入
```java
    @RestController
    public class JobController{
        @Autowired
        private JobComponent myJobComponent = new JobComponent();

        System.out.println(myJobComponent.getJob());
    }
```

# 需要注意的问题
> 注意：属性值中如果有转义，需要使用双引号包裹

```yaml
    person:
        address: "US\LA"
```