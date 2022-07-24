# 使用Docker部署Node

## step1
创建step1文件夹，使用koa创建一个基本项目
```
npm int -y
npm i koa
```
编写node代码
```javascript
const Koa = require('koa');
const app = new Koa();
app.use(ctx => {
    ctx.body = 'Hello Docker!';
});
app.listen(3000);

```
Dockerfile
```docker
# 使用node12的镜像
FROM node:12
# 移动当前文件到app目录下
ADD . /app/
# 进入到app目录下，类似cd
WORKDIR /app
# 对外暴露端口
EXPOSE 3000
# 启动
CMD ["node", "app.js"]

```
这样一个简单的node镜像就做好了
我们还可以加一个.dockerignore
在ADD的时候就不会忽略了