---
title: 记php Session使用
date: 2020-08-22 15:03:39
tags:
- 学习
- PHP
categories:
- 学习 
---
# php Session
* PHP session 变量用于存储关于用户会话的信息

* session是服务器端用于验证用户权限的一把钥匙，存于服务端，在进行数据交互时使用的，我们经常遇到的登录失效这一类场景，就是因为session在中间起到了作用。

## 作用
Session是在服务端保存的一个数据结构，用来跟踪用户的状态，这个数据可以保存在集群、数据库、文件中；   
### 与之相似有`饼干`
Cookie是客户端保存用户信息的一种机制，用来记录用户的一些信息，也是实现Session的一种方式。


## 使用案例
```PHP
/**
     * 判断是否已登录，未登录就结束脚本执行
     * @param bool $die 未登录是否结束脚本
     * @return array|false
     * @author xiaotao
     */
    static public function isLogin($die = true){
        session_start();  //首先必须启动会话。
        if ( !isset($_SESSION['user']) ){
            if( $die ){
                die(ExecuteResult::make(-1, null, '未登录'));
            }else{
                return false;
            }
        }else{
            return $_SESSION['user'];
        }
    }
}
```