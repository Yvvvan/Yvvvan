---
layout:     post
title:      "B站视频相关API"
subtitle:   "以及在Wordpress下简单运用"
date:       2021-09-26
author:     "Yvan"
header-img: "img/in-post/wordpress/wp-bg.jpg"
header-mask: 0.3
header-img-credit: "Dok Sev from Pixabay"
header-img-credit-href:  "pixabay.com/de/users/doki7-646987/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=581849"
no-catalog: false
mathjax: true
category: "code"
tags:
    - Wordpress
    - 前端
    - Javascript
    - Html
    - PHP
typora-root-url: ..
typora-copy-images-to: ..\img\in-post\wordpress
---
> 需求：<br/>
>1.播放列表自动更新UP上传的视频，不需要手动添加链接。<br/>
>2.实现视频切换。


## 1 播放器
> 需求：添加播放器

bilibili 提供的iframe代码  

```html
<!-- html -->
<iframe src="//player.bilibili.com/player.html?aid=56315372&bvid=BV1h441137Uc&cid=99753609&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"></iframe>
```
1. 在src后加上 <span style="color: green">&high_quality=1</span> 可以切换视频清晰度，但目前好像被限制在720P。
2. 添加 宽度和高度 参数 <span style="color: green">width="100%" height="500px"</span>， 调整合适的单位。
3. <span style="color: green">aid/bvid/cid</span> 满足一个即可

## 2 获取视频信息
>需求：获取指定UID用户 个人主页 - 投稿 - TA的视频 页面下的视频。

通过<span style="color: hotpink">Javascript</span>或<span style="color: hotpink">PHP</span>，使用以下<span style="color: green">API</span>，返回<span style="color: hotpink">JSON</span>。JSON中有视频aid，封面图，简介，标题等信息。 
```html
https://api.bilibili.com/x/space/arc/search?mid=UID
```
例如使用<span style="color: hotpink">PHP</span> curl：
```php
// php
$a = "https://api.bilibili.com/x/space/arc/search?mid=8047632";
$ch = curl_init($a);
curl_setopt($ch, CURLOPT_HEADER, 0);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, TRUE);
curl_setopt($ch, CURLOPT_USERAGENT, 'Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.1; Trident/6.0)');
$f = curl_exec($ch);
$obj = json_decode($f);
$vlist = $obj->{'data'}->{'list'}->{'vlist'};
```
<span style="color: hotpink">PHP</span>获取信息后可以存入<span style="color: hotpink">SQL</span>，或者通过<span style="color: green">Wordpress</span>的<span style="color: green">shortcut 插件 (XYZ PHP Code)</span>直接<span style="color: green">echo</span>字符串类<span style="color: green">string</span>给<span style="color: hotpink">Javascript</span>或者<span style="color: hotpink">Html</span>。

## 3 更新播放窗口
>需求：在播放器中播放指定视频/最新视频

1. <span style="color: green">\<iframe\></span>外的<span style="color: green">\<div\></span> 添加id，以便<span style="color: hotpink">Javascript</span>获取
2. 返回<span style="color: hotpink">JSON</span>后，通过<span style="color: hotpink">Javascript</span>, 替换<span style="color: hotpink">html</span>中整个<span style="color: green">iframe</span>。*(如果是给iframe添加id，并替换src，有可能不能更新视频内容)

```javascript
// javascript
// get json
// ...
// get new information
var current_aid = aids[i];
var current_title = titles[i]
// set link
var prefix = "https://player.bilibili.com/player.html?aid=";
var suffix = "&page=1&high_quality=1";
var loc = prefix.concat(current_aid).concat(suffix);
// update
document.getElementById('videoTitle').innerHTML = current_title;
document.getElementById('videoFrame').innerHTML = '<iframe src="'+loc+'" width="100%" height="100%" marginwidth="0" marginheight="0" scrolling="auto" align="middle"></iframe>';
```
可以在<span style="color: hotpink">刷新页面</span>时，或者<span style="color: hotpink">onclick</span>的时候调用。

## 4 设置播放列表
>需求：<br/>
>1. 通过JSON得到所有的视频封面图，用于制作播放列表。<br/>
>2. 点击列表中的视频，激活<span style="color: hotpink">onclick</span>事件，执行3中的过程。

* 图片需要在html中添加 才能正常显示

```html
<!-- html -->
<meta name="referrer" content="never">
```

* 设置一个有id的<span style="color: green">\<div\></span>
* 在<span style="color: hotpink">Javascript</span>中，用<span style="color: green">for循环</span>在<span style="color: green">\<div\></span>中添加图片并设置标题和链接。

```javascript
// javascript
var videos = '';
for (var i = 0; i < all_aids.length-1; i++) {
      videos +='<div ><a href="javascript:changeVideo('+i+');"><img src=" '+ pics[i]+'"/><h3> '+titles[i]+'</h3></a></div><hr>';
}
document.getElementById("videoWrapper").innerHTML = videos;
```
