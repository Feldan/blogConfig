---
title: $_SESSION
date: 2020-08-24 19:16:16
tags:
- 学习
- PHP
categories:
- 学习
- PHP
---
# 超全局数组$_SESSION

## $_SESSION是什么
GatewayWorker中的超全局数组`$_SESSION`和PHP自身的`$_SESSION`功能基本相同。每个client_id对应一个`$_SESSION`数组，`$_SESSION`数组中可以保存对应客户端的会话数据，对应的client_id的后续请求可以直接使用这个数组中的数据，而不用去反复读取存储。

## $_SESSION使用场景
例如客户端链接GatewayWorker后，需要发送验证数据让服务端验证是否合法，一般要传递一次用户名和密码数据，然后在Gateway::onMessage(`$client_id, $message`)中通过查询数据库验证`$message`中的用户名密码是否正确，如果正确就可以将用户的uid写入到`$_SESSION`中如`$_SESSION['uid']=$uid;`，那么当这个`client_id`再次发来数据时，要判断这个客户端是否是被验证过的，就可以用`$_SESSION['uid']`是否被设置来判断。

## $_SESSION实现原理

`Gateway/Worker`中，每个客户端的`$_SESSION`数据是存储在`Gateway`进程内存中的，每次`Gateway`进程转发消息给`BusinessWorker`进程时，都会顺便携带上对应客户端的`$_SESSION`数据给`BusinessWorker`进程，这时`BusinessWorker`进程就能使用`$_SESSION`了。而当`$_SESSION`数据有更改时，`BusinessWorker`会将新的`$_SESSION`数据传递给`G`ateway`进程进行保存。

## 示例
```php
class Events
{
    public static function onMessage($client_id, $data)
    {
        // data={"type":"login", "uid":"666"}
        $data = json_decode($data, true);
        // 如果没有$_SESSION['uid']说明客户端没有登录
        if(!isset($_SESSION['uid']))
        {
            // 消息类型不是登录视为非法请求，关闭连接
            if($data['type'] !== 'login')
            {
                return Gateway::closeClient($client_id);
            }
            // 设置session，标记该客户端已经登录
            $_SESSION['uid'] = $data['uid'];
        }
    }

}
```
```php
$_SESSION['user'] = 0;
echo $_SESSION['user'];
```
```php
$_SESSION['user']['a'] = 1;
$_SESSION['user']['b'] = 2;
echo $_SESSION['user']['a'];
echo $_SESSION['user']['b'];
```
```php
$_SESSION['admin']['a'] = 11;
$_SESSION['admin']['b'] = 22;
```
[文章转载自](http://doc2.workerman.net/session.html)