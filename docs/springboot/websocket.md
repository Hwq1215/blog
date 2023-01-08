# SpringBoot整合WebSocket
> WebSocket是一种在单个TCP连接上进行全双工通信的协议。WebSocket使得客户端和服务器之间的数据交换变得更加简单，允许**服务端主动向客户端推送数据**。在WebSocket API中，浏览器和服务器只需要完成一次握手，两者之间就直接可以创建**持久性的连接，并进行双向数据传输**。

# 配置
## maven依赖
```xml
   <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-websocket</artifactId>
  </dependency>
```
## WebSocket
```java
package com.config;

import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.boot.web.servlet.ServletContextInitializer;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.socket.server.standard.ServerEndpointExporter;
import org.springframework.web.util.WebAppRootListener;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;

@Configuration
@ComponentScan
@EnableAutoConfiguration
public class WebSocketConfig implements ServletContextInitializer {

    @Bean
    public ServerEndpointExporter serverEndpointExporter() {
        System.out.println("我被注入了");
        return new ServerEndpointExporter();
    }

}
```
## 使用
使用`@ServerEndPoint注解`和`@Component`注解一个类，实现客户端和服务端的多种通信
```java
package com.example.websocketdemo.serve;

import javax.websocket.OnClose;
import javax.websocket.OnMessage;
import javax.websocket.OnOpen;
import javax.websocket.Session;
import javax.websocket.server.PathParam;
import javax.websocket.server.ServerEndpoint;
import org.springframework.stereotype.Component;

import java.io.IOException;
import java.util.concurrent.ConcurrentHashMap;

@ServerEndpoint("/web/socket/{userid}")
@Component
public class CalService {
    //定义一个静态的map，用来存放每个客户端对应的MyWebSocket对象
    private static ConcurrentHashMap<String,CalService> webSocketMap = new ConcurrentHashMap<>();
    private Session session;
    private String userid;

    @OnOpen
    public void onOpen(Session session, @PathParam("userid") String userid) throws Exception{
        if(webSocketMap.get(userid)!=null) {
            webSocketMap.get(userid).sendMessage("您的账号已在其他地方登录");
            webSocketMap.get(userid).session.close();
        }else {
            webSocketMap.put(userid, this);
        }
        System.out.println("连接成功:" + userid + "进来了，session编号是" + session.getId());
    }

    @OnClose
    public void onClose(Session session){
        System.out.println(session.getId() + "退出了");
    }

    @OnMessage
    public void onMessage(String message, Session session) throws Exception{
        System.out.println("收到消息：" + message);
    }

    //传给对应session的用户消息
    public void sendMessage(String message) throws IOException {
        this.session.getBasicRemote().sendText(message);
    }

    //发送信息给指定ID用户，如果用户不在线则返回不在线信息给自己
    public void sendMessageTo(String message, String toUserid) throws IOException {
        if(webSocketMap.get(toUserid)!=null) {
            webSocketMap.get(toUserid).sendMessage(message);
        }else {
            this.sendMessage("用户不在线");
        }
    }

    //发送信息给所有在线用户
    public void sendMessageAll(String message) throws IOException {
        for (String key : webSocketMap.keySet()) {
            webSocketMap.get(key).sendMessage(message);
        }
    }
}
```

# 使用自动注入@Autowired报NullPointerException
> 在@ServerEndpoint注解的WebSocket的处理类中不能直接使用@AutoWired注入,这里我们采用一个SpringUtil工具包手动注入
##### SpringUtil
```java
package com.config;

import org.springframework.beans.BeansException;
import org.springframework.context.ApplicationContext;
import org.springframework.context.ApplicationContextAware;
import org.springframework.stereotype.Component;

@Component
public class SpringUtil implements ApplicationContextAware {
    private static ApplicationContext applicationContext;

    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        SpringUtil.applicationContext = applicationContext;

    }

    public ApplicationContext getApplicationContext(){
        return applicationContext;
    }

    public static Object getBean(String beanName){
        return applicationContext.getBean(beanName);
    }

    public static <T> T getBean(Class<T> clazz){
        return (T)applicationContext.getBean(clazz);
    }

}
```
##### 手动注入
```java
 @Autowired
    private ServicerMapper servicerMapper = SpringUtil.getBean(ServicerMapper.class);
```
