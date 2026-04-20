# 问题与简答

## PHP 篇

### 链式调用实现

类定义一个内置变量，让类中其他定义方法可访问到

### 异常处理

set_exception_handler — 设置用户自定义的异常处理函数

使用 try / catch 捕获

### 异步阻塞与异步非阻塞的理解

异步阻塞：打电话问老板有没有某书（调用），老板说你先挂电话，有了结果通知你（异步），你挂了电话后（结束调用）, 除了等老板电话通知结果，什么事情也不做（阻塞）。

异步非阻塞：打电话问老板有没有某书（调用），老板说你先挂电话，有了结果通知你（异步），你挂电话后（结束调用），一遍等电话，一遍嗑瓜子。（非阻塞）

### 如何实现异步调用

```php
$fp = fsockopen("blog.maplemark.cn", 80, $errno, $errstr, 30);
if (!$fp) {
    echo "$errstr ($errno)<br />\n";
} else {
    $out = "GET /backend.php  / HTTP/1.1\r\n";
    $out .= "Host: blog.maplemark.cn\r\n";
    $out .= "Connection: Close\r\n\r\n";
    fwrite($fp, $out);
    /*忽略执行结果
    while (!feof($fp)) {
        echo fgets($fp, 128);
    }*/
    fclose($fp);
}
```

### 多进程同时写一个文件

加锁、队列

### PHP 进程模型，进程通讯方式，进程线程区别

消息队列、socket、信号量、共享内存、信号、管道

### PHP 支持回调的函数，实现一个

array_map、array_filter、array_walk、usort

is_callable + callbacks + 匿名函数实现

### 发起 HTTP 请求有哪几种方式，它们有何区别

cURL、file_get_contents、fopen、fsockopen

### php for while foreach 迭代数组时候，哪个效率最高

### 弱类型变量如何实现

PHP 中声明的变量，在 zend 引擎中都是用结构体 zval 来保存，通过共同体实现弱类型变量声明

### PHP 拓展初始化

- 初始化拓展

```shell
$ php /php-src/ext/ext_skel.php --ext
```

- 定义拓展函数

zend_module_entry 定义 Extension name 编写 PHP_FUNCTION 函数

- 编译安装

```shell
$ phpize $ ./configure $ make && make install
```

### 如何获取扩展安装路径

### 垃圾回收机制

引用计数器

### Trait

自 PHP 5.4.0 起，PHP 实现了一种代码复用的方法，称为 trait

### yield 是什么，说个使用场景 yield、yield 核心原理是什么

一个生成器函数看起来像一个普通的函数，不同的是普通函数返回一个值，而一个生成器可以yield生成许多它所需要的值。

yield核心原理: PHP在使用生成器的时候，会返回一个Generator类的对象。每一次迭代，PHP会通过Generator实例计算出下一次需要迭代的值。简述即yield用于生成值。

yield使用场景：读取大文件、大量计算。

yield好处：节省内存、优化性能

### traits 与 interfaces 区别 及 traits 解决了什么痛点

### 如何 foreach 迭代对象、如何数组化操作对象 $obj[key]、如何函数化对象 $obj(123);

### Swoole 适用场景，协程实现方式

Swoole 是一个使用 C++ 语言编写的基于异步事件驱动和协程的并行网络通信引擎，为 PHP 提供协程、高性能网络编程支持。提供了多种通信协议的网络服务器和客户端模块，可以方便快速的实现 TCP/UDP服务、高性能Web、WebSocket服务、物联网、实时通讯、游戏、微服务等，使 PHP 不再局限于传统的 Web 领域。


协程可以简单理解为线程，只不过这个线程是用户态的，不需要操作系统参与，创建销毁和切换的成本非常低，和线程不同的是协程没法利用多核 cpu 的，想利用多核 cpu 需要依赖 Swoole 的多进程模型。
在底层实现上是单线程的，因此同一时间只有一个协程在工作，协程的执行是串行的。
采用 CSP 编程模型，即不要以共享内存的方式来通信，相反，要通过通信来共享内存。
swoole4.0采用双栈方式，通过栈桢切换来实现协程；即遇到IO等待就切换到。

#### swoole的进程模型

同一台主机上两个进程间通信 (简称 IPC) 的方式有很多种，在 Swoole 中使用了 2 种方式 Unix Socket 和 sysvmsg。

swoole启动后会生成master进程、reactor线程、worker进程、task进程以及manager进程

master进程是一个多线程进程，会生成多个reactor线程
reactor线程负载网络监听、数据收发
work进程处理reactor线程投递的请求数据
task进程处理work进程投递的任务
manager进程用于管理work进程和task进程

### PHP 数组底层实现 （HashTable + Linked list）

PHP 数组底层依赖的散列表数据结构，定义如下（位于 Zend/zend_types.h）。

数据存储在一个散列表中，通过中间层来保存索引与实际存储在散列表中位置的映射。

由于哈希函数会存在哈希冲突的可能，因此对冲突的值采用链表来保存。

哈希表的查询效率是o（1），链表查询效率是o（n）；因此PHP数据索引速度很快；但是相对比较占用空间。

### Copy on write 原理，何时 GC

### 如何解决 PHP 内存溢出问题

### ZVAL

### HashTable

### PHP7 新特性

标量类型声明、返回值类型声明、通过 define() 定义常量数组、匿名类、相同命名空间类一次性导入

### PHP7 底层优化

ZVAL 结构体优化，占用由24字节降低为16字节

内部类型 zend_string，结构体成员变量采用 char 数组，不是用 char*

PHP 数组实现由 hashtable 变为 zend array

函数调用机制，改进函数调用机制，通过优化参数传递环节，减少了一些指令

### PSR 介绍，PSR-1, 2, 4, 7

### Xhprof 、Xdebug 性能调试工具使用

### 字符串、数字比较大小的原理，注意 0 开头的8进制、0x 开头16进制

### BOM 头是什么，怎么除去

WINDOWS自带的记事本，在保存一个以 UTF-8 编码的文件时，会在文件开始的地方插入三个不可见的字符（0xEF 0xBB 0xBF，即BOM）；它是一串隐藏的字符，用于让记事本等编辑器识别这个文件是否以UTF-8编码。
去除方法：$result = trim($result, "\xEF\xBB\xBF");

### 模板引擎是什么，解决什么问题、实现原理（Smarty、Twig、Blade）


### 写一个函数，尽可能高效的从一个标准 URL 中取出文件的扩展名
parse_str,explode
