
## 兼容性

> v1.x.x 与 v2.x.x 不兼容

## 哔哩哔哩设置

> 风控太严，只能选择授权的方式，参考 [Web端Cookie刷新](https://socialsisteryi.github.io/bilibili-API-collect/docs/login/cookie_refresh.html)。

### 打开浏览器登录哔哩哔哩

![登录哔哩哔哩](../images/b-login.png)

### 复制cookie

![复制cookie](../images/b-cookie.png)

### 复制ac_time_value

![复制ac_time_value](../images/ac_time_value.png)

### 注意

这样子相当于登录了，复制后要清理哔哩哔哩浏览器记录，否则会与本插件冲突。

### 在线获取ac_time_value和cookie
> 因为是使用vercel部署的，所有显示登录位置应该是在美国。
[在线获取](https://b-login.vercel.app)
<br>


## 忘记密码

### 进入数据卷目录

```shell
[root@centos7 ~]# docker volume inspect podcast2
[
    {
        "CreatedAt": "2024-03-23T19:57:47+08:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/podcast2/_data",
        "Name": "podcast2",
        "Options": null,
        "Scope": "local"
    }
]
[root@centos7 ~]# cd /var/lib/docker/volumes/podcast2/_data
[root@centos7 _data]# ls
cert  config  database  logs  plugin  resources  tmp
[root@centos7 _data]# cd config/
```

### 修改config.json

```shell
#改成true
{"initUserNameAndPassword":true}
```

### 重启后将恢复默认用户名和密码

> 用户名: admin  
> 密码: 123456

## 开启HTTPS

> 目前仅支持通过上传证书和密钥文件来实现

### 文件格式要求

```shell
# 证书文件格式必须是crt
# 密钥文件格式必须是key
```

### 重启后并以https访问

<br>

## 更新podcast2

> 数据保留

```shell
# 停止容器
docker stop podcast2

# 删除容器
docker rm podcast2

# 删除本地镜像
docker rmi yajuhua/podcast2:latest

# 拉取最新镜像
docker pull yajuhua/podcast2:latest

# 创建新的容器
docker run -id --name=podcast2 \
-p 8088:8088 \
--restart=always \
--mount source=podcast2,destination=/data \
yajuhua/podcast2:latest
```

## 重新开始

> 如果使用最新版都无法解决，可以试试删除数据

```shell
# 停止容器
docker stop podcast2

# 删除容器
docker rm podcast2

# 删除本地镜像
docker rmi yajuhua/podcast2:latest

# 删除数据
docker volume rm podcast2

# 拉取最新镜像
docker pull yajuhua/podcast2:latest

# 创建新的容器
docker run -id --name=podcast2 \
-p 8088:8088 \
--restart=always \
--mount source=podcast2,destination=/data \
yajuhua/podcast2:latest
```
## Invidious API
yt-dlp可能会出现[Sign in to confirm you’re not a bot. This helps protect our community](https://github.com/yt-dlp/yt-dlp/issues/10128)导致无法下载的情况。
目前只能通过设置invidious API进行下载，下面是invidious API列表，找一个能有用的设置即可。
- https://redirect.invidious.io/
- https://api.invidious.io/
## 插件bug或失效

由于插件并非使用官方接口，存在不稳定性。若发现插件失效，请提交[issues](https://github.com/yajuhua/podcast2/issues/new/choose)

---