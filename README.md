# drone-gogs-CI-CD
### drone + gogs 构建CI/CD #
参考网址：https://blog.csdn.net/zl1zl2zl3/article/details/80359282


## 1.下载 gogs drone镜像 ##


    #docker pull dron/agent
# 
    #docker pull  drone/drone
# 
    #docker pull  gogs/gogos

## 2.接下来启动三个server，记得将里面的ip地址改成本机的。 ##

### 2.1启动gogs server

    #docker run --name=gogs -p 10022:22 -p 3000:3000 -v /tmp/gogs:/data gogs/gogs
![](https://i.imgur.com/qc4Eyy3.png)


# 补充：
## 在Linux系统上安装Compose ##

### 1 运行此命令以下载最新版本的Docker Compose： ##

    #sudo curl -L "https://github.com/docker/compose/releases/download/1.22.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

###  2 对二进制文件应用可执行权限：
    #sudo chmod +x /usr/local/bin/docker-compose



###（可选）为 和shell 安装命令完成。bashzsh

### 3 测试安装。
    #docker-compose --version

### 2.2 drone的docker-compose.yml


    version: '2'
    services:
      drone-server:
    image: drone/drone:latest
    ports:
      - 8000:8000
      - 9000:9000
    volumes:
      - /tmp/drone:/var/lib/drone/
    environment:
      - DRONE_OPEN=true
      - DRONE_HOST=http://192.168.131.32:9000
      - DRONE_GOGS=true
      - DRONE_GOGS_URL=http://192.168.131.32:3000
      - DRONE_SECRET=test
      drone-agent:
    image: drone/agent:latest
    command: agent
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    depends_on:
      - drone-server
    environment:
      - DRONE_SERVER=192.168.131.32:9000
      - DRONE_SECRET=test
      - DRONE_DEBUG=true

### 运行命令！

    #docker-compose  up 

图形界面如下：
![](https://i.imgur.com/kG9njhj.png)

![](https://i.imgur.com/USfrPvw.png)


到此为止CI/CD环境搭建完成！

总结：
注意项目版本中使用镜像的版本和容器通信还有就是Docker三辆马车的结合使用。