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

#### 如果是vue等动态加载的网页，可能会出显刷新不是index页面而无法访问的问题
将
```nginx
location / {
		root   html;
        index  index.html index.htm;
}
```
改成
```nginx 
location / {
		root   html;
        index  index.html index.htm;
         
        try_files $uri $uri/ /index.html;
}
```
# yum卸载了怎么办，记一次踩坑
[可以看看这篇](https://blog.csdn.net/weixin_40605941/article/details/122985982)
> 预计你应该做了以下类似的操作，我的建议是再做一次，把python和yum都卸载了，然后再重新安装一遍
```
rpm -qa|grep python|xargs rpm -ev --allmatches --nodeps ##强制删除已安装程序及其关联
 
whereis python |xargs rm -frv  ##删除所有残余文件 ##xargs，允许你对输出执行其他某些命令
 
whereis python ##验证删除，返回无结果
```

> 现在，你只有rpm一个包管理器
### 首先我们需要安装python2，因为yum是基于python2的
```bash
cd /usr/local/src

wget https://registry.npmmirror.com/-/binary/python/2.7.15/Python-2.7.15.tgz

tar -zxvf Python-2.7.15.tgz

cd Python-2.7.15

./configure

make && make install
```

你可能涉及的包
http://mirrors.163.com/centos/7.9.2009/os/x86_64/Packages/python-urlgrabber-3.10-10.el7.noarch.rpm
http://mirrors.163.com/centos/7.9.2009/os/x86_64/Packages/python-iniparse-0.4-9.el7.noarch.rpm
http://mirrors.163.com/centos/7.9.2009/os/x86_64/Packages/yum-metadata-parser-1.1.4-10.el7.x86_64.rpm
http://mirrors.163.com/centos/7.9.2009/os/x86_64/Packages/yum-3.4.3-168.el7.centos.noarch.rpm
http://mirrors.163.com/centos/7.9.2009/os/x86_64/Packages/python-libs-2.7.5-89.el7.x86_64.rpm
http://mirrors.163.com/centos/7.9.2009/os/x86_64/Packages/python-2.7.5-89.el7.x86_64.rpm 
http://mirrors.163.com/centos/7.9.2009/os/x86_64/Packages/python-devel-2.7.5-89.el7.x86_64.rpm
http://mirrors.163.com/centos/7.9.2009/os/x86_64/Packages/python-rpm-macros-3-34.el7.noarch.rpm 
http://mirrors.163.com/centos/7.9.2009/os/x86_64/Packages/python-srpm-macros-3-34.el7.noarch.rpm
http://mirrors.163.com/centos/7.9.2009/os/x86_64/Packages/python2-rpm-macros-3-34.el7.noarch.rpm
http://mirrors.163.com/centos/7.9.2009/os/x86_64/Packages/python-pycurl-7.19.0-19.el7.x86_64.rpm
http://mirrors.163.com/centos/7.9.2009/os/x86_64/Packages/rpm-python-4.11.3-45.el7.x86_64.rpm
http://mirror.centos.org/centos/7/updates/x86_64/Packages/rpm-python-4.11.3-48.el7_9.x86_64.rpm
