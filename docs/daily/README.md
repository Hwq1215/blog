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


