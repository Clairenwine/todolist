# todo-list

## 运行地址

http://47.92.245.184:8080

## 运行界面
![Alt text](src/assets/31672928917_.pic_hd.jpg)

## 项目部署

```
FROM nginx

MAINTAINER LLJ

RUN rm /etc/nginx/conf.d/default.conf

ADD default.conf /etc/nginx/conf.d/

COPY dist/ /usr/share/nginx/html/
```

## 本地运行

```
npm install
npm run serve
```

