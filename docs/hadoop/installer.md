# Hadoop的在虚拟机上的配置和安装
## 前期准备

- 平台：Windows10
-  虚拟机软件：VMware 16.2.2
- [Linux镜像：CentOS 7.2.1511](http://mirror.nsc.liu.se/centos-store/7.2.1511/isos/x86_64/CentOS-7-x86_64-DVD-1511.iso)


## 安装虚拟机
- 虚拟机：3台
- 安装步骤：
    1. 打开Vmware 
    2. 创建新的虚拟机 
    3. 典型 
    4. 选择镜像 
    5. 创建用户 
    6. 配置界面

### 网络配置
- 点击VMware首选项的**虚拟网络编辑器**

    ![编辑虚拟网卡](./imgs/Snipaste_2024-10-24_13-37-03.png)

- 检查VMnet8(NAT模式)的**子网IP**和**子网掩码**

    ![检查](./imgs/Snipaste_2024-10-24_13-38-52.png)

    这里的子网IP是`192.168.220.0`,子网掩码是`255.255.255.0`

### 配置的选择
- 内存：6GB+
- 硬盘：20GB+
- CPU：至少1核心
- 网络：NAT模式（VMnet8）保证3台机器在同一个虚拟网络下

![配置](./imgs/Snipaste_2024-10-24_13-39-39.png)

### 开机后需要做的一些事
#### 设置网络
- 先找到接口名称   

![接口名称](./imgs/Snipaste_2024-10-24_14-51-37.png)

这里是eno16777736
去到相应的接口进行配置的修改
```bash
  vi /etc/sysconfig/network-scripts/ifcfg-eno16777736  
```
填入相应的网络配置
```vim
IPADDR=192.168.220.130 
PREFIX=24             
GATEWAY=192.168.220.2  
DNS1=8.8.8.8           
DNS2=114.114.114.114   
```
重启网络
```bash
systemctl restart network
```
确认虚拟机的网络连接
```bash
ping 192.168.220.131 # 在192.168.220.130 ping 192.168.220.131
```
确定外部网络的连接和DNS工作是否正常
```bash
ping www.baidu.com
```

#### 设置主机名
打开hosts文件
```bash 
vim /etc/hosts
```
在hosts的内容清空，根据节点的IP换成以下内容
```vim
192.168.220.130 master
192.168.220.131 slave1
192.168.220.132 slave2
```
重启网络
```bash
systemctl restart network
```
确认hosts文件是否生效
```bash
ping master
```
### 新建hadoop用户
新建用户
```bash
sudo useradd hadoop
sudo passwd hadoop
```
给用户root权限（在root下设置）
```bash
visudo
```
再打开的文件的指定位置输入
```bash
hadoop ALL=(ALL) ALL
```

![visudo](./imgs/Snipaste_2024-10-24_15-34-47.png)
## 环境配置
### 所有节点都需要的配置
#### java安装和环境变量配置
建立一个java目录
```bash
mkdir /usr/java
```
获得jdk的压缩包
```bash
wget https://mirrors.tuna.tsinghua.edu.cn/Adoptium/8/jdk/x64/linux/OpenJDK8U-jdk_x64_linux_hotspot_8u422b05.tar.gz --no-check-certificate
```
移动压缩包到建立的文件夹
```bash
mv ./OpenJDK8U-jdk_x64_linux_hotspot_8u422b05.tar.gz /usr/java
cd /usr/java
```

解压文件夹
```bash
tar -zxvf  ./OpenJDK8U-jdk_x64_linux_hotspot_8u422b05.tar.gz
```
打开~/.bashrc配置java环境变量
```bash
vi ~/.bashrc
```
在文件末尾加入以下内容
```bash
export JAVA_HOME=/usr/java/jdk8u422-b05
export CLASSPATH=.:$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$PATH:$JAVA_HOME/bin
```

#### 关闭防火墙
停止防火墙
```bash
sudo systemctl stop firewalld
```
关闭开机启动
```bash
sudo systemctl disable firewalld
```

#### 配置节点间相互无密码登录
生成密钥
```bash
ssh-keygen
```
传递密钥
```bash
ssh-copy-id hadoop@slave1
```
**注意master，slave1，slave2三个都要配置一遍**

### master节点的mysql的配置与安装
1. [配置阿里云yum源](https://blog.csdn.net/g310773517/article/details/140321025)
2. 添加mysql的信息
    ```bash
    yum clean all
    yum makecache
    yum update
    wget http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm
    yum localinstall ./mysql57-community-release-el7-8.noarch.rpm
    yum repolist enabled | grep "mysql.*community.*"
    yum install mysql-community-sever
    ```
3. 启动mysql，设置mysql开机启动
    ```bash
    systemctl start mysqld
    systemctl enable mysqld
    ```
4. 设置mysql密码
    - 查看临时密码
        ```bash 
        grep 'temporary password' /var/log/mysqld.log
        ```
    - 用临时密码登录mysql
        ```bash 
        mysql
        ```
    - 进入mysql后
        ```sql
        SET PASSWORD = PASSWORD('Hadoop@123');
        CREATE USER 'hadoop'@'%' IDENTIFIED BY 'Hadoop@123';
        grant all on *.* to 'root'@'%' identified by 'Hadoop@123';
        grant all on *.* to 'hadoop'@'%' identified by 'Hadoop@123';
        FLUSH PRIVILEGES;
        quit;
        ```

### 时区同步

#### master节点
安装ntp,打开ntp配置文件
```bash
yum install ntp
chkconfig ntpd on
vim /etc/ntp.conf
```
将服务器设为以下内容，将其他server注释
```bash
server cn.pool.ntp.org iburst
```

重启查看状态
```bash
service ntpd restart 
systemctl status ntpd.service
```
#### 其他节点
打开chrony配置
```bash
vi /etc/chrony.conf
```
设置服务器，将其他server注释
```bash
server 192.168.220.130 iburst 
```
重启查看状态
```bash
service chronyd restart
chronyc sources
```
## Hadoop的安装和配置
- [jdk1.8](https://mirrors.tuna.tsinghua.edu.cn/Adoptium/8/jdk/x64/linux/)
- [Hadoop2.7.3](https://archive.apache.org/dist/hadoop/core/hadoop-2.7.3/)   
- MySQL57

获得Hadoop tar格式压缩包后，解压压缩包
```bash 
tar 
```

