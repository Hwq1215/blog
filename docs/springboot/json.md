# SpringBoot 接受 json 和发送 json
> springboot常常需要收发json格式的请求；就springboot作为后端来说，前端放在body的数据通常是json格式的；而springboot微服务也常用json格式数据。
## 阿里的包pom.xml
```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.28</version>
</dependency>
```
## 接收 json
### 第一种方法：
前端传回来的 json
```js
{
    "key0":1,
    "key1":"my name"
}
```
Controller中的接口写法
```java
    @PostMapping("/receive")
    public String receive(@RequestBody JsonObject jsonObject){  //使用JsonObject对象接收
        Object object = jsonObejct.getObject("key0");           //利用JsonObject的get方法获得对象
        String string = jsonObejct.getString("key1");           //同理                   
    } 
```
### 第二种方法：
前端传回来的 json
```js
{
    "userId":123456,
    "password":"myname",
    "username":"Tom"
}
```
先写一个User类
```java
    
    @Data
    @AllArgsConstructor
    @NoArgsConstructor
    public class User {
    private Integer userId;
    private String password;
    private String username;

    public Integer getUserId() {
        return userId;
    }

    public void setUserId(Integer userId) {
        this.userId = userId;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }
}
```
再在Controller中写接口
```java
    @PostMapping("/receive")   
    public String receive(@RequestBody User user){   //用已注入的对象接收
        Integer userId  = user.getUserId();   
        String password = user.getPassword();   
        String username = user.getUsername();
    }         
```

## 发送 json
### 使用@RestController 注解
```java
    @RestController
    public String send(){   
        Integer userId  = user.getUserId();   
        String password = user.getPassword();   
        String username = user.getUsername();
        User user = new User(userId,password,username);
        return user;         //这里会自动将对象封装成json
    }
```