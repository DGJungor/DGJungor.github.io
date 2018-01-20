---
title: PDO的基本操作
date: 2017-12-29 14:40:53
tags: [PDO,数据库]
---
PHP操作MySQL数据库方式有三种：
   *1. mysql 最原始的、纯过程化的 如连接： mysql_connect(主机名，账号，密码);
      mysql_query();
       
   2. mysqli 改进版的、兼容过程化和面向对象化操作
      如：连接： mysqli_connect(主机名，账号，密码，库名) //过程化
               new mysqli(主机名，账号，密码，库名)       //面向对象
               
   *3. PDO 通用的，兼容其他数据库 ， 纯面向对象方式
      如： 连接： new PDO(DSN,账号，密码);
      
      选择PDO的原因：跨数据库，带预处理（防sql注入）、支持事务操作
        

<!-- more -->

## mysqli系列函数只能操作mysql/mariadb数据库、mysqli拓展不能操作其他数据库(oracel/sqlsever)。



## PDO （PHP Data Object ）提供了一个数据访问抽象层，意味着，不管使用哪种数据库，都可以用相同的方法来查询和获取数据。
        
=============================================================================
   PDO--PHP Data Objects
=============================================================================

   PDO的环境配置：开启支持PDO
      在php.ini配置文件中开启：
            extension=php_pdo.dll  ；5.1以后版本不用加这个
            extension=php_pdo_mysql.dll
            
   在PDO操作中涉及到类：PDO、PDOStatement(预处理对象)、PDOException（异常类）


一、 PDO类的构造方法：
---------------------------------------------------------
  PDO __construct( string dsn 
      [, string username 
      [, string password 
      [, array driver_options]]] );
      
 其中:dsn数据库连接信息如“mysql:host=localhost;dbname=库名”
     dsn的格式：”驱动名:host=主机名;dbname=库名“
      username：用户名
      password:密码
      driver_options：配置选项：
      如： PDO::ATTR_PERSISTENT=>true,是否开启持久链接
      *PDO::ATTR_ERRMODE=>错误处理模式：(可以是以下三个)(3)
      PDO::ERRMODE_SILENT:不报错误（忽略）(0)
      PDO::ERRMODE_WARNING:以警告的方式报错(1)
      *PDO::ERRMODE_EXCEPTION：以异常的方式报错(推荐使用)。(2)

$pdo =  new PDO("mysql:host=localhost;dbname=lamp36db","root","root");
$pdo->setAttribute(PDO::ATTR_ERRMODE,PDO::ERRMODE_EXCEPTION);
其他方法：
--------------------------------------------------------
1. query($sql); 用于执行查询SQL语句。返回PDOStatement对象
2. exec($sql);  用于执行增、删、改操作，返回影响行数；
3. getAttribute(); 获取一个"数据库连接对象"属性。
4. setAttribute(); 设置一个"数据库连接对象"属性。
5. beginTransaction 开启一个事物（做一个回滚点）
6. commit  提交事务
7. rollBack    事务回滚操作。 
8. errorCode   获取错误码   
9. errorInfo   获取错误信息   
10.lastInsertId  获取刚刚添加的主键值。
11.prepare 创建SQL的预处理，返回PDOStatement对象
12.quote   为sql字串添加单引号。


预处理对象PDOStatement对象：
=============================================
我们可以通过PDO的方法来获取PDOStatement：
 1.PDO的query（查询sql）方法获取，用于解析结果集
 2.PDO的prepare(SQL)方法获取，用于处理参数式sql并执行操作。

PDOstatement对象的方法：
----------------------------------------------------------------
1、fetch() 返回结果集的下一行，结果指针下移，到头返回false 。
   参数：    PDO::FETCH_BOTH (default)、：索引加关联数组模式
              PDO::FETCH_ASSOC、     ：关联数组模式
              PDO::FETCH_NUM、          ：索引数组模式
         PDO::FETCH_OBJ、          ：对象模式
         PDO::FETCH_LAZY          ：所有模式（SQL语句和对象）
         
2、fetchAll() 通过一次调用返回所有结果，结果是以数组形式保存
       参数：PDO::FETCH_BOTH (default)、
      PDO::FETCH_ASSOC、
      PDO::FETCH_NUM、
      PDO::FETCH_OBJ、
      PDO::FETCH_COLUMN表示取指定某一列，
      如：$rslist = $stmt->fetchAll(PDO::FETCH_COLUMN,2);取第三列
3、execute()    负责执行一个准备好了的预处理语句 
4. fetchColumn()返回结果集中下一行某个列的值
5. setFetchMode()设置需要结果集合的类型
6. rowCount()      返回使用增、删、改、查操作语句后受影响的行总数
7. setAttribute()为一个预处理语句设置属性
8. getAttribute()获取一个声明的属性
9. errorCode()     获取错误码
10. errorInfo() 获取错误信息
11. bindParam() 将参数绑定到相应的查询占位符上
    bool PDOStatement::bindParam ( mixed $parameter , mixed &$variable [, int $data_type [, int $length [, mixed $driver_options ]]] ) 其中： $parameter：占位符名或索引偏移量 &$variable:参数的值，需要按引用传递也就是必须放一个变量
    其中参数:$data_type:数据类型PDO::PARAM_BOOL/PDO::PARAM_NULL/PDO::PARAM_INT/PDO::PARAM_STR/
                 PDO::PARAM_LOB/PDO::PARAM_STMT/PDO::PARAM_INPUT_OUTPUT
         $length：指数据类型的长度 $driver_options：驱动选项。
12. bindColumn() 用来匹配列名和一个指定的变量名，这样每次获取各行记录时，会自动将相应的值赋给变量。
13. bindValue() 将一值绑定到对应的一个参数中
14. nextRowset() 检查下一行集
15. columnCount() 在结果集中返回列的数目
16. getColumnMeta() 在结果集中返回某一列的属性信息
17. closeCursor() 关闭游标，使该声明再次执行


在PDO中参数式的SQL语句有两种(预处理sql)：
   1.insert into stu(id,name) value(?,?);  //？号式（适合参数少的）     
   2.insert into stu(id,name) value(:id,:name);       // 别名式(适合参数多的)
在PDO中为参数式SQL语句赋值有三种：
   1.使用数组 
    $stmt->execute(array("lamp1404","qq2"));
    $stmt->execute(array("id"=>"lamp1404","name"=>"qq2"));    
   2.使用方法单个赋值
    $stmt->bindValue(1,"lamp1901");   
    $stmt->bindValue(2,"qq2");
    $stmt->execute();

    $stmt->bindValue(":id","lamp1901",PDO::PARAM_STR);     //带指定类型
    $stmt->bindValue(":name","qq2",PDO::PARAM_STR);
    $stmt->execute();
    
   3. 使用方法绑定变量
    $stmt->bindParam(":id",$id);     
    $stmt->bindParam(":name",$name);
    $id="lamp1401";
    $name="qq2";
     $stmt->execute();
    
事务处理
-----------------------------------------------    
   事务：将多条sql操作（增删改）作为一个操作单元，要么都成功，要么都失败。-----
   
4.  PDO对事务的支持
      第一：被操作的表必须是innoDB类型的表（支持事务）
         MySQL常用的表类型：MyISAM(非事务)查询速度快、InnodB（事务型）安全性高
         //更改表的类型为innoDB类型
         mysql> alter table stu engine=innodb;
            Query OK, 29 rows affected (0.34 sec)
            Records: 29  Duplicates: 0  Warnings: 0
         //查看表结构
         mysql> show create table stu\G;   
            
      第二：使用PDO就可以操作数据库了
            使用到了PDO中的方法：
               beginTransaction 开启一个事物（做一个回滚点）
               commit    提交事务
               rollBack   事务回滚操作。 
      
      使用情况：当做多条sql语句处理时(增删改)，要求是都必须成功。   