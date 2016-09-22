---
layout: post
title: php中将xml转成json
category: tech
tags: php xml json
---

在做底层接口，php对接一个老项目，对方返回的是xml，解析起来还是很麻烦的。还是转成json简单易用。

SimpleXML 函数是 PHP 核心的组成部分，它允许我们把 XML 转换为对象。

假定对方http返回的内容是 utf-8 编码的$response，我们返回的json格式给上层使用

	$body = $response->body;

	$xml = simplexml_load_string($body);
	$result = json_decode(json_encode($xml, JSON_UNESCAPED_UNICODE), true);

	return response()->json($result, $response->status);
	

参考资料：

* [PHP SimpleXML 函数](http://www.w3school.com.cn/php/php_ref_simplexml.asp)
* [PHP: 预定义常量 - Manual](http://php.net/manual/zh/json.constants.php)