---
title: js操作cookie
date: 2018-07-22 07:34:29
tags: code
categories: code
---
使用js处理cookie
<!--more-->
```javascript
//写cookies
function setCookie(name,value) 
{ 
    var Days = 30; 
    var exp = new Date(); 
    exp.setTime(exp.getTime() + Days*24*60*60*1000); 
    document.cookie = 
    name + "="+ escape (value) + ";expires=" + exp.toGMTString(); 
} 
//读取cookies
function getCookie(name) 
{ 
    var arr,reg=new RegExp("(^| )"+name+"=([^;]*)(;|$)");
 
    if(arr=document.cookie.match(reg)){
        return unescape(arr[2]); 
    }
    else {
        return null; 
    }
} 
//删除cookies 
function delCookie(name) 
{ 
    var exp = new Date(); 
    exp.setTime(exp.getTime() - 1); 
    var cval=getCookie(name); 
    if(cval!=null) {
        document.cookie= name + "="+cval+";expires="+exp.toGMTString(); 
    }
} 

```

封装一下，好看点
```javascript
var cookie_operation = (function(){
    var get = function(name){ 
        var arr,reg=new RegExp("(^| )"+name+"=([^;]*)(;|$)");
        if(arr=document.cookie.match(reg)){
            return unescape(arr[2]); 
        }
        else {
            return null; 
        }
    };
    var del = function(name) { 
        var exp = new Date(); 
        exp.setTime(exp.getTime() - 1); 
        var cval=getCookie(name); 
        if(cval!=null) {
            document.cookie= name + "="+cval+";expires="+exp.toGMTString(); 
        }
    };
    var set = function(name,value) { 
        var Days = 30; 
        var exp = new Date(); 
        exp.setTime(exp.getTime() + Days*24*60*60*1000); 
        document.cookie = name + "="+ escape (value) + ";expires=" + exp.toGMTString(); 
    } 
    return {
        set : set,
        get : get,
        del : del
    }
})();

```