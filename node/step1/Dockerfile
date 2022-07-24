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
