# CentOS7中安装Nginx

## 安装支持类库

### 安装make

```shell
yum -y install gcc automake autoconf libtool make
```

### 安装g++

```shell
yum install gcc gcc-c++
```

## 安装Nginx

### 选择安装文件目录

```shell
cd /usr/local/src
```

### 安装PCRE库

```shell
wget ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.39.tar.gz 
tar -zxvf pcre-8.37.tar.gz
cd pcre-8.34
./configure
make
make install
```

### 安装zlib库

```shell
wget http://zlib.net/zlib-1.2.11.tar.gz
tar -zxvf zlib-1.2.11.tar.gz
cd zlib-1.2.11
./configure
make
make install
```

### 安装openssl

```shell
wget https://www.openssl.org/source/openssl-1.0.1t.tar.gz
tar -zxvf openssl-1.0.1t.tar.gz
yum -y install openssl openssl-devel
```

### 安装nginx

```shell
wget http://nginx.org/download/nginx-1.1.10.tar.gz
tar -zxvf nginx-1.1.10.tar.gz
cd nginx-1.1.10
./configure
make
make install
```

## 启动Nginx

### 查看端口是否占用

```shell
netstat -ano|grep 80
```

### 启动

```shell
/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
```

### 停止

```shell
# 查看进程号，杀死进程
ps -ef|grep nginx
从容停止：kill -QUIT 进程号   快速停止：kill -TERM/-INT 进程号  强制停止：kill -9 进程号
```

### 重启

```shell
# 方法一
/usr/local/nginx/sbin/nginx -s reload
# 方法二
kill -HUP 进程号
```



## 验证配置文件

### 验证nginx配置文件是否正确

> * #### 进入nginx安装目录sbin下，输入命令./nginx -t
>
>   ```shell
>   # 说明配置文件正确的显示
>   nginx.conf syntax is ok
>   nginx.conf test is successful
>   ```

