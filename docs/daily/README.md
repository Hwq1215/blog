# 23-11-16
今天写了一个脚本，用于检测并重新检查网络连接，如果网络连接失败，则会自动重新连接校园网。

check_and_recon.bat:
```cmd
@echo off
setlocal

REM Define the log file
set log_file=error.log

REM Define the target URL and login command
set target_url=http://172.30.21.100/api/account/login
set login_command=curl "%target_url%" ^
  -H "Accept: */*" ^
  -H "Accept-Language: en,zh-CN;q=0.9,zh;q=0.8,en-GB;q=0.7,en-US;q=0.6,zh-TW;q=0.5" ^
  -H "Connection: keep-alive" ^
  -H "Content-Type: application/x-www-form-urlencoded" ^
  -H "Origin: http://172.30.21.100" ^
  -H "Referer: http://172.30.21.100/tpl/whut/login.html?acip=172.30.1.223&acname=WHUT-Bras-ME60-A&ip=10.84.132.1&nasId=52&userip=10.84.132.1&wlanacname=" ^
  -H "User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/16.0 Safari/605.1.15 Edg/119.0.0.0" ^
  -H "X-Requested-With: XMLHttpRequest" ^
  --data-raw "username=310620&password=qaz11097969..^%^3D&switchip=&nasId=52&userIpv4=&userMac=&captcha=&captchaId=" ^
  --compressed ^
  --insecure        

REM Loop to check network connectivity every 10 seconds
:LOOP
REM Ping a reliable server (e.g., Google DNS)
ping -n 3 8.8.8.8 > nul

REM Check the errorlevel to determine if the network is connected
if %errorlevel% neq 0 (
    echo [%date% %time%] Network not connected. Performing login... >> %log_file%
    %login_command%
    echo [%date% %time%] Login completed. >> %log_file%
) else (
    echo [%date% %time%] Network is already connected.
)

REM Wait for 10 seconds before checking again
timeout /nobreak /t 10 > nul
goto LOOP

endlocal
```
check_and_recon.vbs:
```vbs
Set objShell = CreateObject("WScript.Shell")
objShell.Run "cmd /c check_and_recon.bat", 0, True  
```

对桌面进行了改造，使得任务栏居中，使用的工具为TaskbarX 
路径C:\Program Files\TaskbarX

学习了如何使用路由器KMS激活Office
https://www.bilibili.com/read/cv21108793/

买了一些NFC贴纸，准备做一个利用IOS快捷指令和Window互传的剪切板的工具。
| 平台 | 方案 |
| :---: | :---: |
| IOS | 快捷指令 |
| Windows | Python脚本（flask） | 

# 23-11-17
今天学习了`DES算法`一种过时的对称加密算法，准备晚上的信息安全考试
还有之前学过的`RSA算法`，一种非对称加密算法，用于数字签名和密钥交换
Step:
1. 选择两个大素数p和q，计算n=pq
2. 计算欧拉函数φ(n)=(p-1)(q-1)
3. 选择一个整数e，使得1<e<φ(n)，且e与φ(n)互质
4. 计算e关于模φ(n)的模反元素d，即d*e=1(mod φ(n))
5. 公钥为(n,e)，私钥为(n,d)
6. 加密：c=m^e(mod n)
7. 解密：m=c^d(mod n)


# 23-11-18
1. 今天完成了`clipboard ios to win 1.0`的构建
2. 完成了线性动画圆角气泡框，使用—transparent参数实现`tkinter.root`的透明，然后用`tkinter.canvas`实现圆角和淡入淡出动画
调用关系 RoundLabel->frame
3. 实现了文本的互传，win到ios的图像传单向传送。pywin32中有获取剪切和清除设置剪切板内容的功能，pillow包里面有个grap剪切板图像的功能，能直接获得win剪切板的图像，获得一个`IMAGE类`，此时用类的save方法放到一个`BytesIO类`中，声明png图像类，最后返回`BytesIO类`的getValue，使用flask Response类包装
4. 苹果这边使用快捷指令的获取URL向电脑的Web服务器发起访问。用NFC触发这一开关

# 23-11-19
今天是实现了IOS到WIN的图像发送，在快捷指令中加入一个表单元素（文件），在WIN的server接收，通过判断剪切板文本`IOS_text_content`有无元素，再继续之后的图像表单元素读取，因为IOS剪切板没内容，图像没东西，会报错，当然，服务器端也要用`image/*`判断文件是否为图像，否则认为不是图像，不进行处理，这样就可以实现文本和图像的互传了。

# 23-11-20
今天解决两个问题
1. 网络ip更换问题，有可能我们连接的是热点，不是通过路由器在同一局域网内，所以将flask的`host`改成`0.0.0.0`，这样，任何客户端和服务端处于同一局域网，并知道服务端的ip地址，就可以访问了。但是IOS的快捷指令要改东西，用分支判断现在的网络是哪个，并更改访问的url中的ip地址，这样就可以实现在不同网络下的互传了，同时，服务端的地址也要为静态地址。
2.  IOS接收文本换行问题，之前直接把字符串作为`Response`发送，换行符被代替成空格，于是将`Response`包装一下，
```python
response_content = Response(WIN_text_content,mimetype='text/plain')
```




