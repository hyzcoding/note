# jquery

## 简介

> 轻量级javascript库

## CDN

```html
<script src="https://apps.bdimg.com/libs/jquery/2.1.4/jquery.min.js">
<script src="https://lib.sinaapp.com/js/jquery/2.0.2/jquery-2.0.2.min.js">
```

## 使用

```jquery
$(function(){
 
   // 开始写 jQuery 代码...
 
});
```

## ajax

```javascript
htmlobj=$.ajax({url:"/jquery/test1.txt",async:false});

$.ajax({
    //请求方式
    type : "POST",
    //请求的媒体类型
    contentType: "application/json;charset=UTF-8",
    //请求地址
    url : "http://127.0.0.1/admin/list/",
    //数据，json字符串
    data : JSON.stringify(list),
    //请求成功
    success : function(result) {
        console.log(result);
    },
    //请求失败，包含具体的错误信息
    error : function(e){
        console.log(e.status);
        console.log(e.responseText);
    }
});
```

