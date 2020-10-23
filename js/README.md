# 简介

## javascript

- ajax

  ```javascript
  var xmlhttp;
  if (window.XMLHttpRequest) {// code for IE7+, Firefox, Chrome, Opera, Safari
      xmlhttp=new XMLHttpRequest();
  } else {// code for IE6, IE5
      xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
  }
  // 异步时
  xmlhttp.onreadystatechange=function() {
      if (xmlhttp.readyState==4 && xmlhttp.status==200)  {
          document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
      }
  }
  //  2.设置请求方式和请求地址； 
  // 判断请求的类型是POST还是GET
  if (type === 'GET') {
      // true 异步 ，false 同步
      xmlhttp.open(type,url+"?t=" +str,true);
      //  3.发送请求;
      xmlhttp.send();
  }else{
      xmlhttp.open(type,url,true);
      // 注意：在post请求中，必须在open和send之间添加HTTP请求头：setRequestHeader(header,value);
      xmlhttp.setRequestHeader("Content-type","application/x-www-form-urlencoded"); 
      //  3.发送请求;
      xmlhttp.send(str);
  }   
  xmlhttp.send();
  ```

  

## ES6

## typescript



## Design System

