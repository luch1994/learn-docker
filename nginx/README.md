# Docker使用nginx

## 1. 使用命令行工具

### 1.1 拉取官方镜像
```bash
docker pull nginx
```

### 1.2 启动nginx镜像
```bash
mkdir www
echo 'hello docker!' >> www/index.html
docker run -p 80:80 -v $PWD/www:/usr/share/nginx -d nginx
```
创建www文件夹
输出文本hello docker!到www文件夹下面的index.html
-p 80:80 表示将本地的80端口映射到镜像的80端口，第一个80表示本地的80端口，第二个80表示镜像的80端口
-v 表示绑定一个卷，可以理解为，当访问镜像里的```/usr/share/nginx```文件夹的时候即为访问当前问卷家的www文件夹
-d nginx 表示后台运行nginx容器

### 1.3 查看进程
```
docker ps
docker ps -a
```

### 1.4 进入容器的伪终端
上面我们 docker ps -a 打印了容器的信息，可以拿到容器的id（CONTAINER ID），假设容器的id是 ```821b24a7847a```
我们执行下面的命令可以进入容器的终端
```bash
docker exec -it 821b24a7847a bin/bash
```

默认的nginx配置在 /etc/nginx/conf.d/default.conf
```
cat /etc/nginx/conf.d/default.conf
```

我们可以看到
```
location / {
    root   /usr/share/nginx/html;
    index  index.html index.htm;
}
```
配置的root文件夹是/usr/share/nginx/html，我们上面绑定卷的操作就可以理解了

执行exit退出伪终端
```bash
exit
```

### 1.5 停止和删除镜像
停止
```bash
docker stop 容器id
```
删除
```
docker rm 容器id
```


## 2. 使用dockerfile定制镜像

### 2.1 基本使用
在根目录下创建文件Dockerfile，输入下面的代码

```docker
FROM nginx:latest
RUN echo '<h1>hello docker!</h1>' > /usr/share/nginx/html/index.html
```
当前目录执行
```bash
docker build -t mynginx .
docker run -p 80:80 -d mynginx
```

### 2.2 映射文件夹到容器内的文件夹
Dockerfile如下
```docker
FROM nginx:latest
RUN copy www /usr/share/nginx/html/
```
当前目录执行
```bash
docker build -t mynginx .
docker run -p 80:80 -d mynginx
```
和上述效果一样，不过当前是把www文件夹绑定到容器内的/usr/share/nginx/html/文件夹下

### 2.3 自定义nginx配置
我们创建一个nginx.conf的文件，配置如下

```nginx
server {
    listen 80;
    charset utf-8;
    location / {
        root /usr/share/nginx/html;
        try_files $uri $uri/ /index.html;
    }
}
```
编写Dockerfile如下
```docker
FROM nginx:latest
COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY www /usr/share/nginx/html/
```

命令行执行
```bash
docker build -t mynginx .
docker run -p 80:80 -d mynginx
```

## 3. 其他更高阶的使用待补充
