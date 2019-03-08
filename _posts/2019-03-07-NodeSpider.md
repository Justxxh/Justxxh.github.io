---
layout: post
title: "[Node 爬虫]快速制作自己的桌面壁纸"
date: 2019-03-07 23:50:40
author: "JustX"
header-img: "assets/img/dhxy1.jpg"
tags: [Node]
comments: true
---

## 背景

这几天家用台式机莫名卡顿，直至某晚女友看到我wallpaper engine那骚气的壁纸时说了一句：真丑，我帮你换几张壁纸吧。 就在她一顿百度之后 准备另存为，突然就各种卡死崩溃，强制关掉浏览器之后再次操作依旧如此，我一顿排查无果，恰好最近觉得电脑很奇怪，最后只能启用终极方案—— 重装大法，经过一顿操作 安装各种驱动，常用软件及游戏的下载，最终搞定，可是细看总觉得哪里不对，猛然想起，原来是这该死的默认壁纸拉低了我的档次，之前心爱的搜狗壁纸也停止维护，主要也是不想安装一堆软件，于是乎开始了壁纸的搜索，女友在知乎上找到很多好看的壁纸，无奈回答里都是直接发图没有网盘链接，看着这191张图片（我怎么会一个个书，当然是是爬虫下载的时候统计的[Smirk]），这要一个个保存得到什么时候去，于是就有了后面的故事...



## 需解决的问题及思路

<section>
通过node写爬虫代码抓取网络图片并下载到本地文件夹，刚好最近也在学习node作为一个练习也是不错的
本次的环境是在Windows下，之后在macbook上也测试过正常运行 只是要注意一下存储路径的问题
思路：

<ul>
	<li>首先创建目录，新建app.js文件,安装相关依赖</li>
</ul>

```js
	npm i superagent cheerio fs
```

```js
	package.json依赖如下
		
	   "dependencies": {
           "cheerio": "^1.0.0-rc.2",
           "fs": "0.0.1-security",
           "superagent": "^4.1.0"
         }

```
<ul>
	<li>通过superagent发送http请求获取资源，拿到html</li>
    <li>cheerio 是node.js的jquery库通过这个解析DOM，</li>
    <li>fs 就用于新建文件夹和保存文件</li>
</ul>
<section>
大致流程就如上所说，现在找一个自己想要爬取的网站，我这里用的是
    <mark><a href="https://www.zhihu.com/question/37914433" target="_blank">知乎的某个回答</a> 在此感谢回答者提供这些好看的壁纸！</mark>代码很简单，都有写注释，下面贴出代码
</section>

```js
/**
 * JustX
 * 2019-03-07 22:48:03
 */
const superagent = require('superagent');//发送http请求
const cheerio = require('cheerio');//获取dom
const fs = require('fs') //创建文件、文件夹

// 知乎回答壁纸地址
const url='https://www.zhihu.com/question/37914433', // 图片爬取地址
      path = 'D:\\', //  指定路径 \ 转义符
      imgfolder = 'wallpaper'; // 文件夹名
      imgSelectorName = 'data-original'// 获取图片arr 原图地址属性 data-original

superagent.get(url) 
.set({  //设置请求头
    "Connection":"keep-alive",
})
.end((err,res) => {
    if(err){
        console.log(err);
        return ;
    }
    // 判断是否存在
    fs.exists(path + imgfolder + '/', function(exists) {
        if(!exists){
            console.log('创建文件夹')
            //创建放图片的文件夹
            fs.mkdir(path + imgfolder + '/', (err) => {
              if (err) {
                 console.log(err)
              }
            })
        }
        // 获取dom对象
        let $ = cheerio.load(res.text)
        // 循环
        let num = 0
        fs.exists(path + imgfolder + '/', function (exists) {
            if (exists) {
                files = fs.readdirSync(path + imgfolder + '/');
            }
            let beforeNum = files.length // 之前存在的图片数
            console.log(beforeNum)
            $('img.origin_image').each(function (index, el) {
                // 限制条件 大于1920*1080
                // if($(el).attr('width') >= 1920 && $(el).attr('height') >= 1080){
                num++
                var imgurl = $(el).attr(imgSelectorName), // 获取图片arr 原图地址属性 data-original
                    index = index + beforeNum
                imgnam = 'pic' + index; //拿到图片的标题
                // 利用request模块保存图片
                superagent(imgurl).pipe(fs.createWriteStream(path + imgfolder + '/' + imgnam + '.jpg'))
                // }
            })
            console.log(`此次下载${num}张,共${beforeNum + num}张`)
        });

    });
    
})
```

## 成果

{% capture images %}
    http://justx.cn:8088/assets/img/0308-1.png
{% endcapture %}
{% include gallery images=images caption="" cols=2 %}
<p>Windows壁纸设置为相册 随机模式即可，运行程序几秒拥有自己的壁纸集</p>
<em><mark>tips：</mark>以上为本次爬虫实践记录，初次实践可能存在的问题还望各位指出，也希望能给正在学习的朋友提供一点思路，最后感谢各位浏览</em>