---
title: THINKPHP5 缓存应用的实例
date: 2018-01-31 11:42:23
tags: [缓存,ThinkPHP5]
categories: [PHP,框架,ThinkPHP5]
---

合理的运存缓存 可是加快网站加载速度  减低网站服务器的负荷

<!-- more -->


### 在THINKPHP5中 缓存的配置放在了config.php文件中 代码如下

```php

// +----------------------------------------------------------------------
// | 缓存设置
// +----------------------------------------------------------------------
'cache'                   => [
    // 驱动方式
    'type'   => 'File',
    // 缓存保存目录
    'path'   => CACHE_PATH,
    // 缓存前缀
    'prefix' => '',
    // 缓存有效期 0表示永久缓存
    'expire' => 0,
],

```

### 如何设置缓存

1. 可以使用静态方法
```php
Cache::set('key',$value,3600);//存储缓存

Cache::get('key');//获取缓存
```

2. 也可是先实例化再调用
```php
$cache_model=new Cache();//实例化缓存模型

$info=$cache_model->get($cache_key);//获取缓存

$cache_model->set($cache_key,$info,$cache_expire_time);//设置缓存
```

### 一个简单的实例

```php
<?php
//实例化
$cacheModel = new Cache();

//设置缓存kuy
$cacheKey = 'userInfo123';

//缓存时间600秒
$cacheExpireTime = 600

//获取缓存
$userInfo = $cacheModel->get($cacheKey);

//判断是否从缓存中获取到数据
if($userInfo){

    //缓存中有数据
    return $userInfo;
}else {

    //没有获取到数据 重新从别的模型获取数据 放入缓存
    $userInfo = $model->getIndo();

    //添加数据到缓存中
    $cacheModel->set($cacheKey,$userInfo,$cacheExpireTime);

}

```

cache的其他操作
'''php
<?php

// 针对数值类型的缓存数据，可以使用自增自减操作
Cache::inc('name');     // name自增（步进值为1）
Cache::inc('name',3);   // name自增（步进值为3）
Cache::dec('name');     // name自减（步进值为1）
Cache::dec('name',3);   // name自减（步进值为3）
Cache::get('name','');  // 表示如果name值不存在，则返回空字符串。
Cache::rm('name');      //删除缓存
Cache::pull('name');    //获取并删除缓存 如果name值不存在，则返回null
Cache::clear();         //清空缓存

'''
