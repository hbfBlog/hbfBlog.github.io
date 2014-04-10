---
layout: post
title: "Rails基础知识"
date: 2014-04-10 06:43:44 +0800
comments: true
categories: rails
keywords: rails
tags: rails
---
积累Rails的小知识。<!--more-->
####***`resources :posts`创建了哪些路由？***
+ 创建的路由可以通过`rake routes`来查看
          posts GET    /posts(.:format)          posts#index
                POST   /posts(.:format)          posts#create
       new_post GET    /posts/new(.:format)      posts#new
      edit_post GET    /posts/:id/edit(.:format) posts#edit
           post GET    /posts/:id(.:format)      posts#show
                PATCH  /posts/:id(.:format)      posts#update
                PUT    /posts/:id(.:format)      posts#update
                DELETE /posts/:id(.:format)      posts#destroy
    *总结起来，基本上只有4个形式，/posts、/posts/new、/posts/:id、/posts/:id/edit，动词有五种get、post、put、patch、delete。*

####***什么是RESTful？***
+ REST="Representation State Transfer",表现层状态转化
1. 每一个URI代表一种资源
2. 客户端和服务器之间，传递这种资源的某种表现层。
3. 客户端通过四个HTTP动词，对服务器端资源进行操作，实现"表现层状态转化"  
简单的可以认为，HTTP是无状态的，REST风格使得通过URL可以传递状态，去除了额外的参数。  
Tips：
  - URI中不要包含动词：/posts/show/1，应该改为/posts/1
  - URI中不要加版本号：/app/1.0/foo，可以在HTTP请求头信息的Accept字段中进行区分Accept: vnd.example-com.foo+json; version=1.0

####***User.find(10) 和 User.find_by_id(10)的区别***
+ `User.find(10)`相当于： `select users.* from "users" where "user"."id" = ? limit 1 [["id", 1]]`;失败后返回ActiveRecord::RecordNotFound异常。
+ `User.find_by_id(10)`相当于: `select users.* form "users" where "user"."id" = 1 limit 1`;失败返回nil。

####***在控制台中执行`user = User.first; user.name = 'wangwei'`后, 如何查看数据库中的user信息?***
+ 使用user.reload,对对象进行重载。

####***rake的作用***
+ `rake routes`： 列出当前应用的所有定义的路由。
+ `rake test`： 运行测试用例。
+ `rake assets：precompile`: 对静态资源进行预编译。
+ `rake stats`：查看应用中代码统计的数据。
+ `rake secret`：生成伪随机key。