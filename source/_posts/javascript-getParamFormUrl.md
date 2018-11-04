---
title: javascript 从浏览器url获取参数 
date: 2018-08-03 14:14:51
tags: javascript
categories: code
password:
---

```javascript
function getParamFromUrl (name) {
          name = name.replace(/[\[]/, "\\\[").replace(/[\]]/, "\\\]");
          var regexS = "[\\?&]" + name + "=([^&#]*)";
          var regex = new RegExp(regexS);
          var results = regex.exec(window.location.href);
          if (results == null) {
              return "";
          } else {
              return results[1];
          }
  } 
  ```  
