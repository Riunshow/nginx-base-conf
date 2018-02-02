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

* [传送门](http://123.207.154.174/mystatus)


### 随机主页

```bash
    location / {
        root    /opt/app/code;
        random_index on;
        index  index.html index.htm;
    }
```
访问 http://ip

* [传送门](http://123.207.154.174)




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

* [传送门](http://123.207.154.174/submodule.html)