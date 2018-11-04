---
title: hexo 自定样式
date: 2018-08-03 14:33:05
tags: css
categories: code 
password:
---
本博客自定样式, 图片使用七牛云的免费存储。
<!--more-->
~~~css

body {
    background:url(http://s5.sinaimg.cn/orignal/006CagVjzy7msKeDaXW94&690);
    background-repeat: no-repeat;
    background-attachment:fixed;
    background-position:50% 50%;
    background-size:100% 100%;
}

.header {
   background: rgba(0,0,0,0.7) none repeat scroll;
}

.posts-expand .page, .content .archive{
    background-color: #fff;
    opacity: 0.9;
    padding: 2rem;
}

.posts-expand .post {
    margin-top: 50px;
    background-color: #fff;
    padding: 2rem;
    opacity: 0.9;
    border-radius: 4px;
}
.posts-expand .post-title, .posts-expand .post-meta {
    text-align: center;
}
.post-button {
    text-align: center;
}

.footer {
    background: rgba(0,0,0,0.5) none repeat scroll;
}

.menu-item-active a {
    background: none;
}

.site-title {
   color: #f0e68c; 
}

.header a {
    color: #f0e68c;
}

.footer  {
    color: #f0e68c;
}

.footer-inner {
    text-align: center;
}
.v {
    background: rgba(0,0,0,0.5) none repeat scroll;
   
}

.v *{
     color: #f0e68c !important;
}

.v .vbtn {
    border: 1px solid #222 !important;
    background: #222 !important;
}

.v .vlist .vcard .vhead .vsys {
    background: #222 !important;
}

::-moz-placeholder { color: #f0e68c; }
::-webkit-input-placeholder { color: #f0e68c; }
:-ms-input-placeholder { color: #f0e68c; }
.pagination span, .pagination a { color: #f0e68c; }

~~~