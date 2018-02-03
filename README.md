# nginx-base-conf

```bash
vi /etc/nginx/conf.d/default.conf
```
### 查看连接状态等
配置中添加
```bash
    location /mystatus {
        stub_status;
    }
```
访问 http://ip/mystatus


### 随机主页

```bash
    location / {
        root    /opt/app/code;
        random_index on;
        index  index.html index.htm;
    }
```
访问 http://ip


### 全局替换内容

```bash
    location / {
        root    /opt/app/code;
        index  index.html index.htm;
        sub_filter '<p>c' '<p> 替换后的内容 CCC';
        sub_filter_once off;
    }
```
访问 http://ip/yourself.html


### 频率限制

* 连接频率限制
```bash
    limit_conn_zone $binary_remote_addr zone=conn_zone:1m;

    location / {
        root    /opt/app/code;
        limit_conn conn_zone 1;
        index  index.html index.htm;
    }
```

* 请求频率限制
```bash
    limit_req_zone $binary_remote_addr zone=req_zone:1m rate=1r/s;

    location / {
        root    /opt/app/code;
        # limit_req zone=req_zone burst=3 nodelay;
        # limit_req zone=req_zone burst=3;
        # limit_req zone=req_zone;
        index  index.html index.htm;
    }
```

### ip 的访问控制

除了这个 ip 都可以访问
```bash
    location ~ /admin.html {
        root   /opt/app/code/;
        deny 182.201.211.62;
        allow all;
        index  index.html index.htm;
    }
```

除了这个 ip 都不可以访问
```bash
    location ~ /admin.html {
        root   /opt/app/code/;
        allow 182.201.211.62;        
        deny all;
        index  index.html index.htm;
    }
```

### 访问控制

输入用户名及密码

```bash
[root@VM_150_136_centos nginx]# htpasswd -c ./auth_conf botai
newpassword:
```
会在当前目录下生成一个 auth_conf,其中的内容
```bash
# 设置的用户名 botai 密码 123456
[root@VM_150_136_centos nginx]# more auth_conf
botai:$apr1$8S/eoJKQ$PmnYn5bqllMC/SjQbcUe3.
```

```bash
    location ~ /admin.html {
        root   /opt/app/code/;
        auth_basic "Auth access test!input your password";
        auth_basic_user_file /etc/nginx/auth_conf;
        index  index.html index.htm;
    }
```
输入正确会跳转,错误则返回401

访问 http://ip/admin.html

<img src="./img/auth.png">