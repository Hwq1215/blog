# API说明
## 关于封装
### 所有的返回json都是以 返回代码 code，信息流 msg，数据data 的封装形式。
### 返回数据(data)为空的返回json都是说明状态的，code = 200为成功，code = 404为失败。
### 每个List对象我都有唯一的代号 id，你可以用此属性进行Get查询。
### 以下的带 "{}"的 Get请求都是可以替换的数据，相当于变量。 

<br></br> 
## 一、首页
- 首页的元素我全部没有在数据库存储，而是静态存储。
> 
1. 返回除公司内容以外的首页内容，包括 tech_content，event_name，strategy_content， strategy_img
```java
    @GetMapping("index/content")
    //返回数据为三个String:  tech_content，strategy_content，strategy_img
    //一个List<String>:  event_name
```

2. 返回要显示的公司信息, 包括 id,  company_name,  detail_message
```java
    @GetMapping("index/company")
    //返回数据为一个List<id, company_name, detail_message>
```
#
## 二、发布
1. 后端获得传来的标志符，表示表格是否从前端成功发入
```java
    @PostMapping("publish/ifSend")
    //返回数据为空
```
2. 返回当前的步骤(未完成，这里的实现方法？)
```java 
    @GetMapping("publish/step")
    //返回数据为整型
```
3. 提交项目的数据
```java
    @PostMapping("publish/summit")
    //返回数据为空
```
#
## 三、项目搜索（常用的是5，6，7。7最方便但是效率不如Get方法）
+ 1的data数据是一个项目的数据
  
+ 除了1外，都是返回List<项目>的数据
+ 项目的属性：
  ```java
    public class DealingProject {
        private Integer id;         //项目编号
        private Integer lowPrice,highPrice; //价格
        private Integer amount;     //处理量
        private String imgPath;     //图片文件路径
        private String port;        //端口
        private String type;        //种类
        private String name;        //项目的名字
        private String demand;      //需求
        private String introduce;   //项目的内容
        private String location;    //项目的地域
        private Integer ripe;        //成熟度
        private String time ;       //项目开始时间
        private String tag1;         //标签1
        private String tag2;         //标签2
        private String tag3;         //标志3
    }
  ```
+ 如果返回空值,返回时 code = 404，msg = "fail:no exist"
  
<br></b>
1. 根据项目的id查询（变量是数字整型）
```java
    @GetMapping("searchProject/byId/{id}")
```
2. 访问所有项目
```java 
    @GetMapping("searchProject/all")
```
3. 根据项目的类型查询（变量是字符串）
```java
    @GetMapping("searchProject/byType/{type}")
```
4. 根据项目的地点查询
```java
    @GetMapping("searchProject/byLocation/{location}")
```
5. **根据项目的关键词模糊匹配查询，包括 项目的name、location、type、tag  （搜索框的位置）**
```java
    @GetMapping("searchProject/byKeyword/{keyword}")
```
6. **和上面类似，但是限定了地点 （城市选择的位置）**
```java
    @GetMapping("searchProject/byKeywordByLocation/{keyword}/{location}")
```
7. **这个是post请求，你把要查询要用到的数据用post请求发送过来（可以发一个或多个条件），分别有四个参数（注意下列参数名字的大小写）：**
   + keyword    //匹配的关键词

   + location   //确定的地点   

   + lowPrice   //最低价格

   + highPrice  //最高价格
 ```java
    @PostMapping("searchProject/byKeywordByLocationByPrice")
 ```
<br></b>
   - 下面的的排序做一个说明，以上2，5，6，7每一次Get得到数据后，后端会缓存最后一次查询的结果，并且根据缓存的数据来排序，如果还没有进行Get，那么目前暂存的是全部的项目数据。 这样你前端就可以完成先搜索再排序或直接全局排序的操作，后端也不用频繁访问数据库。
8. 根据价格升序获得排序后的序列
```java
    @GetMapping(value = {
        "searchProject/order/price" ,
        "searchProject/order/price/asc"
        })
    //两者都可以访问到
```
9. 与8同理，顺序相反
```java
    @GetMapping("searchProject/order/price/dec")
```
10. 与8同理，根据日期升序来排序
```java
     @GetMapping(value = {
         "searchProject/order/time/asc" ,
         "searchProject/order/time"
         })
```
11. 与10相反
```java
    @GetMapping("searchProject/order/time/dec")
```
#
## 四、咨询
- 咨询类我就做了一个post的提交，其他现在感觉需求还没有确定。
1. post提交咨询
```java
    @PostMapping("consult")
    //返回数据为空
```
#
## 五、登录
1. 用户登录
   
```java
   @PostMapping ("login/in")
   //提交的表单数据
   fromdata{
       int useId;
       String password;
   }
```
----
|状态|  返回编码（code）|   返回消息（msg）
|:--|:--|:--
|存在该用户且密码正确|200|user exist and password correct
|存在用户但密码错误|402|user exist but password incorrect
|不存在用户|403|user no exist

2.用户信息暂定
```java
    public class User {
    private Integer userId;
    private String password;
    private String username;
    }
```
#
##  六、收藏
1.用户收藏该项目
```java
    @PostMapping ("collect/new")
    fromdata{
        Integer useId;  //用户id
        Integer projectId;     //项目的id属性
    }
```
2.用户收藏的项目
```java
    @GetMapping ("collect/{userId}")
    //返回数据是List<项目>
```

