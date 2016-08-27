---
layout: post
title: 用 Nokitjs 解决前端开发中的跨域问题 
category: 随笔
published: true
date: 2016-04-13
---

### 问题
在开发一些「单页应用」时，通常会使用 Ajax 和服务器通讯，比如 RESTful API，通常「前端」和「服务端 API」可能是有不同人员在负责，也不在同一个工程下，那么开发过程中就可能会遇到跨域的问题，比如 Chrome 会在 console 中看到这样的错误消息:

```
XMLHttpRequest cannot load http://google.com/. No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin 'http://run.jsbin.io' is therefore not allowed access.
```

浏览器因为安全原因，有「同源策略」不允许「跨域」，有时也会给开发过程带来一点点小麻烦。

<!--more-->

### 常见方法
  

##### 1. Access-Control-Allow-Origin 
目前主流浏览器都支持，通过在服务器的响应头信息中添加 **Access-Control-Allow-Origin** 以声明允许来自那些「域」的跨域请求，比如:
```
Access-Control-Allow-Origin: xxx.xyz
```
也可以允许任何来源的跨域请求
```
Access-Control-Allow-Origin: *
```

很少有场景必须要在「生产环境」使用 *，如果开发环境使用 *，那么在部署到生产环境时，为了安全启见，无论手动还是自动的方式，都需要换成「特定的域」

当然在开发环境也可指定特定的「域」，如上边的 xxx.xyz，那开发过程中就需要每个开发人员添加 host 配置，如下:
```
127.0.0.1 xxx.xyz
```

##### 1. nginx 反向代理
用代理的方式解决的跨域问题，就不要添加什么「响应头」了，用 nginx 搭建一个「用于开发」的 WebServer，然后，我们可以把某些 URL 转发到「目标地址」，然后前端用 ajax 请求同域下的地址，这样自然就不存在「跨域问题」了，nginx 配置大约如下：
```
...
location /api/ {
    rewrite  ^/api/(.*)  /$1 break;
    ...
} 
... 
```

这个方式，需要让每个前端开发人员安装并配置 nginx，虽然可以正好学习 nginx，却还是稍显麻烦。

### 用 Nokitjs 解决问题 
Nokitjs 是一个「A Web development framework」，和 express/koa/hapi 等框架类似，用于开发「Web 应用或网站」，这里不去比较各个框架的优劣，而是去解决「跨域」问题。

Nokitjs 提供了「命令行工具」，在终端中直接使用「Nokit CLI」需要全局安装 Nokit:
```sh
npm install nokitjs -g
```

Nokit CLI 一般用于启动「基于 Nokit 开发的应用」，同时它也能在「指定的目录」启动一个「静态 WebServer」，如下:
```sh
nokit start [端口] [应用目录省略时为当前目录] [其它选项]
```
「其它选项」中有一个 **-pulibc** 选项，可以指定「静态资源目录」，如下命令，将在当前目录启一个「静态 WebServer」
```sh
npm start 8000 -public=./
```

如何解决跨域问题？，还需要一个插件 **nokit-filter-proxy**，接下来用一个实例说明，假如我们有一个工程，结构如下:
```
应用目录
├── dist
├── package.json
└── src
```

**dist** 是「构建工具」Build 的目标目录，**src** 是源码目录，**package.json** 是 NPM 包配置文件。

安装 nokitjs 和 nokit-filter-proxy 并保存到 **devDependencies**

```
npm install nokitjs nokit-filter-proxy --save-dev
``` 

配置 **package.json** 的 **scripts**，如下
```json
...
"scripts": {
    "start": "nokit start 8000 -public=./dist",
    "stop": "nokit stop",
    "restart": "npm stop && npm start",
    ...
}
...
```
现在，「不需要全局安装」 nokitjs，在「应用目录」执行：
```
npm start
```
即可启动一个「静态 WebServer」，将会看到如下提示：
```
[Nokit][L]: Starting...
[Nokit][L]: The server on "localhost:8000" started
```
就可以在浏览器中访问 **http://localhost:8000** 了。

然后配置 **nokit-filter-proxy**，在「应用目录」新建一个文件 **config.json**，写入如下内容：
```
{
    "filters": {
        "^/": "nokit-filter-proxy"
     },
     "proxy": {
        "rules": {
          "^/api/(.*)": "http://xxx.xyz/"
        }
     }
}
```

如上配置，首先注册了 **nokit-filter-proxy**，然后添加了一条转发规则，将所有 **api** 开头的 URL 转发到 **http://xxx.xyz**，比如: 
```
GET /api/user/id
``` 

将会被转发到 

```
GET http://xxx.xyz/user/id
```
可以添加任意多条转发规则，规则越靠后优化级越高。

相比 nginx 省事不少，不需要每个开发人员再安装配置 nginx，可以在获取代码后，直接执行

```
npm install
```
  
完成所有依赖的安装，然后便可以使用 **npm start** 启动 Server，并在浏览器中预览或调试了。

另外，在启动时还可以通过 **config** 选项指定配置文件名，比如
```
nokit start 8000 -public=./dist -config=webserver
```

这样，应用根目录的 **config.json** 就可以换成 **webserver.json** 了。

或许，还希望不同的「环境」转发到不同的「地址」，又或者每个开发人员需要不同转发规则，可以通过 **--env** 指定不同的环境配置，也可以通过「系统环境变量 NODE_ENV」指定，如下
```
nokit start 8000 -public=./dist -env=local
```

或

```
export NODE_ENV=local
```

这样，在应用目录可以建立一个 **config.local.json** 文件，格式和 **config.json** 相同，nokit 会合并这两个文件，相同的配置节「环境配置文件」将覆盖「默认配置文件」的配置。


最后附上相关模块的 GitHub 地址:
1. nokitjs https://github.com/nokitjs/nokit
2. nokit-filter-proxy https://github.com/nokitjs/nokit-filter-proxy


