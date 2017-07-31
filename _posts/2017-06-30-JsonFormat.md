---
layout: master
title:  Json字符串格式化
description:  yuminglang.com,提供web前端、java、linux、数据库相关技术知识共享。
categories: Json
syntax: 12
---
&nbsp;

<html>
<head>
<meta charset="UTF-8">
<title>JsonFormat</title>
<style type="text/css">
*{
margin: 0;
padding:0;
}
textarea{
min-width: 100%;
resize: none;
}
#srcView{
min-height: 30vh;
}
</style>
</head>
<body>
	<textarea id="srcView"
		oninput="javascript:document.getElementById('jsonView').innerHTML=JSON.stringify(JSON.parse(this.value),null,1)"
		onblur="if(this.value.length<1){this.value='请输入需要转换的JSON字符串...'}"
		onclick="if(this.value.substring(0,3)=='请输入'){this.value=''}">请输入需要转换的JSON字符串...离线版在页面源码尾部注释中</textarea>
	<pre><code id="jsonView"></code></pre>
</body>
</html>

<!-- 离线版HTML代码 start-->
<!--
<html>
<head>
<meta charset="UTF-8">
<title>JsonFormat</title>
<style type="text/css">
* {
	margin: 0;
	padding: 0;
	border: 0px solid gray;
}

textarea {
	min-width: 100%;
	resize: none;
}

#srcView {
	min-height: 30vh;
}

#jsonView {
	background-color: gray;
	min-height: 70vh;
}
</style>
</head>
<body>
	<textarea id="srcView"
		oninput="javascript:document.getElementById('jsonView').innerHTML=JSON.stringify(JSON.parse(this.value),null,1)"
		onblur="if(this.value.length<1){this.value='请输入需要转换的JSON字符串...'}"
		onclick="if(this.value.substring(0,3)=='请输入'){this.value=''}">请输入需要转换的JSON字符串...</textarea>
	<textarea id="jsonView" readonly></textarea>
</body>
</html>
-->
<!-- 离线版HTML代码 end-->