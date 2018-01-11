---
title: TP5中同字段where多条件查询
date: 2017-12-25 14:10:21
tags: [ThinkPHP5,框架]
---

### 若要同字段查询多条件 一般用in

``` php
//条件
$where=  [1,2,3,4,5];

//查询
$data = this
	->where('id', 'in',$where)
	->select();
		
```