# Linux的配置
## 用yum install安装nginx
我们都知道,yum 的仓库没有nginx的包,我们要先进行一些操作,再来使用`yum install nginx`命令
#### 首先,我们使用vim命令转到nginx.repo的仓库源文件,如果不存在,该命令也会自动创建这样一个文件夹
```bash
    vim /etc/yum.repos.d/nginx.repo
```

#### 然后,我们在该文件中进行编辑,写入以下内容
```bash
[nginx]
name=nginx
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=0
enabled=1
```

#### 最后,运行`yum install nginx`即可
```bash
yum install nginx
```

#### 安装好以后测试一下 nginx 服务
```bash
service nginx status
```
- 如果在停止服务了
```
nginx is stopped
```

- 如果正常运行
```bash
● nginx.service - nginx - high performance web server
   Loaded: loaded (/usr/lib/systemd/system/nginx.service; disabled; vendor preset: disabled)
   Active: inactive (dead)
     Docs: http://nginx.org/en/docs/
```

#### 再测试一下 nginx 的配置文件：
```bash
nginx -t
```
应该会返回
```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```