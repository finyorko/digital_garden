---
{"type":"技术","title":"[[halo博客部署ssl证书以及反向代理]]","description":null,"aliases":null,"id":null,"author":"fun","category":null,"categories":null,"mathjax":true,"tags":null,"date":"2022-06-16","updated":"2024-04-28T22:10:54.450+08:00","comments":true,"publish":true,"dg-publish":true,"permalink":"/01 学习笔记/技术学习/halo博客部署ssl证书以及反向代理/","dgPassFrontmatter":true,"created":"2024-04-28T22:08:19.612+08:00"}
---

# docker部署halo博客并部署ssl证书以及反向代理

## docker部署halo博客

1. 创建工作目录，博客相关的所有文件均存在`.halo`目录下，故备份也只需要这一个文件夹备份

   ```shell
   # 创建halo博客的工作目录
   mkdir ~/.halo && cd ~/.halo
   ```

2. 下载配置文件，可以根据官方的 [配置参考](https://docs.halo.run/getting-started/config)进行修改

   ```bash
   wget https://dl.halo.run/config/application-template.yaml -O ./application.yaml
   ```

3. 拉取halo的最新镜像

   ```shell
   docker pull halohub/halo:latest
   ```

4. 创建容器

   ```bash
   docker run -it -d --name halo -p 8090:8090 -v ~/.halo:/root/.halo --restart=unless-stopped halohub/halo:latest
   ```

5. 此时可以通过`http://ip:<端口号>`进行访问



## nginx反向代理并配置ssl证书

1. 安装`nginx`

   ```shell
   yum install -y nginx
   ```

   如果没有源，添加nginx源：

   ```
   vi /etc/yum.repos.d/nginx.repo
   在文件中写入以下内容：
   [nginx]
   name=nginx repo
   baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
   gpgcheck=0
   enabled=1
   ```

2. `nginx`配置文件目录在`/etc/nginx`下

   在`/etc/nginx/conf.d`目录下创建`halo.conf`新文件

   ```shell
   vim /etc/nginx/conf.d/halo.conf
   ```

   ```json
   upstream halo {
     server 127.0.0.1:8090;
   }
   server {
       
       listen 80;
   
       server_name amorcode.cn www.amorcode.cn;
   
       client_max_body_size 1024m;
   
       location / {
       
           proxy_set_header HOST $host;
           proxy_set_header X-Forwarded-Proto $scheme;
           proxy_set_header X-Real-IP $remote_addr;
           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
   
           proxy_pass http://127.0.0.1:8090/;
       }
       rewrite ^(.*) https://$host$1 permanent;
   }
   
   server {
       
       listen 443 ssl;
   
       server_name amorcode.cn www.amorcode.cn;
   	# 配置证书的源，已经存放在/usr/local/nginx/conf/cert/下
       ssl_certificate /etc/nginx/cert/6651021_amorcode.cn.pem;
       ssl_certificate_key /etc/nginx/cert/6651021_amorcode.cn.key;
       ssl_session_timeout 5m;
       ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
       ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
       ssl_prefer_server_ciphers on;
   
       location / {
       
           proxy_set_header HOST $host;
           proxy_set_header X-Forwarded-Proto $scheme;
           proxy_set_header X-Real-IP $remote_addr;
           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
   
           proxy_pass http://127.0.0.1:8090/;
       }
       location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|flv|mp4|ico)$ {
           proxy_pass http://halo;
           expires 30d;
           access_log off;
      }
       location ~ .*\.(js|css)?$ {
           proxy_pass http://halo;
           expires 7d;
           access_log off;
      }
   }
   ```
   
   ```shell
   nginx -t #验证nginx配置
   nginx -s reload #重载Nginx配置
   ```
   

## 使博客支持webp图片（完全摘抄的官网）

> https://halo.run/archives/halo-and-webp.html

下面将提供两种 web 服务器的代理方法。

> 此教程以 CentOS 7.x 为例，其他发行版本大同小异。另外，此教程只针对于 Halo，其他 web 程序可能在 config.json 部分有所不同，建议参考仓库的 README。

### 部署 webp_server_go

在 [仓库](https://github.com/webp-sh/webp_server_go) 的 README 中已经大致讲解了部署方法，在这里针对 Halo 详细说明一下。

#### 下载官方编译好的 webp_server_go 二进制文件

> 如果你有能力，也可以自行编译。

新建一个存放二进制文件和 config.json 文件的目录（可自定义）：

```
mkdir /opt/webps

cd /opt/webps
```

下载二进制文件（最新版本请访问 [releases](https://github.com/webp-sh/webp_server_go/releases)）：

```
wget https://github.com/webp-sh/webp_server_go/releases/download/0.1.0/webp-server-linux-amd64 -O webp-server
```

给予执行权限：

```
chmod +x webp-server
```

#### 创建 config.json

```
{
        "HOST": "127.0.0.1",
        "PORT": "3333",
        "QUALITY": "80",
        "IMG_PATH": "/root/.halo",
        "EXHAUST_PATH": "/root/.halo/cache",
        "ALLOWED_TYPES": ["jpg","png","jpeg"]
}
```

参数解释：

- HOST：一般不修改。
- PORT：webp_server_go 的运行端口。
- QUALITY：转换质量，默认为 80%。
- IMG_PATH：固定格式，/运行 Halo 的用户名/.halo
- EXHAUST_PATH：固定格式，/运行 Halo 的用户名/.halo/cache
- ALLOWED_TYPES：需要转换的格式

#### 使用 systemd 进行状态管理

创建 service 文件：

```
vim /etc/systemd/system/webps.service
```

写入：

```
[Unit]
Description=WebP Server
Documentation=https://github.com/n0vad3v/webp_server_go
After=nginx.target

[Service]
Type=simple
StandardError=journal
AmbientCapabilities=CAP_NET_BIND_SERVICE
WorkingDirectory=/opt/webps
ExecStart=/opt/webps/webp-server --config /opt/webps/config.json
ExecReload=/bin/kill -HUP $MAINPID
Restart=always
RestartSec=3s


[Install]
WantedBy=multi-user.target
```

需要注意的是，ExecStart 命令中的程序路径和配置文件路径一定要正确，结合你的实际情况填写。

然后执行：

```
systemctl daemon-reload
systemctl enable webps.service
systemctl start webps.service
```

查看运行状态：

```
systemctl status webps.service
```

如果没有问题，那么会输出以下日志：

```
WebP Server is running at 127.0.0.1:3333
```

### 使用 Nginx 进行代理

> 如果你的 Halo 是使用 Nginx 反向代理的话。

#### 修改 halo.conf

在 server 节点添加：

```
location ^~ /upload/ {
        proxy_pass http://127.0.0.1:3333;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_hide_header X-Powered-By;
        proxy_set_header HOST $http_host;
        add_header Cache-Control 'no-store, no-cache, must-revalidate, proxy-revalidate, max-age=0';
}
```

重载 Nginx 配置：

```
# 检查配置文件是否有问题
nginx -t 

nginx -s reload
```

### 使用 Caddy 进行代理

> 如果你的 Halo 是使用 Caddy 反向代理的话。

#### 修改 Caddyfile

在你域名节点下添加：

```
proxy /upload/ localhost:3333 {
  transparent
}
```

重启 Caddy：

```
service caddy restart
```

教程完毕，下面讲一下如何验证是否生效。

### 验证是否生效

![Screen Shot 2020-03-02 at 11.47.08 PM](https://halo.run/upload/2020/3/Screen%20Shot%202020-03-02%20at%2011.47.08%20PM-045cd797c4e3464a9d6919447a7d9e7f.png)

注意看 `Type` 列，图片的返回格式已经变成了 webp，而且图片大小已经远远降低，那么说明你的配置已经成功了。have fun!
