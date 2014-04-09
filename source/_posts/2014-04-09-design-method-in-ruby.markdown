---
layout: post
title: "Ruby知识积累"
date: 2014-04-09 08:32:02 +0800
comments: true
categories: ruby
keywords: ruby
tags: ruby
---
积累自己在学习Ruby过程中遇到的疑惑。<!--more-->
####***Ruby的优缺点？***  
  + 优点：
    + 解释型执行，方便快捷
    + 语法简单、优雅
    + 完全面向对象
    + 内置正则式引擎，适合文本处理
    + 自动垃圾收集机制
    + 跨平台和高度可移植性
    + 有优雅、完善的异常处理机制
    + 拥有很多高级特性:例如操作符重载、Mix-ins、特殊方法等等

  + 缺点：
    + 解释型语言，所以速度较慢
    + 静态检查比较少

####***require, load,autoload,include的区别是什么?***  
  * require.load用于文件，如*.rb；include用于包含一个文件中的模块。 
  * require一般情况下用于加载库文件，而load则用于加载配置文件。  
  * require只加载一次，load可加载多次  
  * autoload(name,file_name)在模块名称name第一次被访问的时候，加载file_name。  

####***"中国 2014".size和"中国 2014".bytesize有什么区别？***  
+ 输出结果分别为：size=7，bytesize=9；
+ String#size,String#length返回的是字符个数；
+ String#bytesize返回的字节个数。  

####***string 和 symbol有什么区别？***
+ string定义后可以改变,即使内容相同的string，其object_id也不同；
+ symbol定义后不可以进行改变，同一个符号的object_id是唯一的。  
> 镐头书中这样描述：
> symbol are simply constant names that you don't have to predeclare and that are guaranteed to be unique.There is no need to assign some kind of value to a symbol--Ruby takes care of that for you.

####***Ruyb中有哪些设计模式？***
+ Singleton(单件)模式  
 - 以库的形式实现了此模式，使用singleton库的方法是先`require 'singleton'`然后在类中`include Singleton`。
+ Proxy(代理)模式
 - tempfile库的实现使用了此模式，使得该库在不用指定文件名的情况下就可以生成临时的工作文件。
 - 可以通过Ruby中delegate库来实现此模式，tempfile库也是使用此库来实现的。
 - method_missing方法的使用，也是代理模式的机制的一个体现。
+ Visitor(访问者)模式
 - Ruby中使用此模式来实现使用块进行循环的抽象化。
+ Prototype（原型）模型
>Prototype模式：明确一个实例作为要生成对象的种类模型，通过复制该实例来生成新的对象。
 - Ruby虽然是类模式的语言，也拥有支持原型模式编程的功能。
  - 复制对象的clone方法
  - 给个别对象增加方法的特异方法功能
  - 给个别对象增加一组功能的extend方法
+ Template Method(模板方法)模式
 >Template Method模式:在父类的一个方法中定义算法的框架，其中几个步骤的具体内容则留给子类来实现。
 - Ruby中最大限度的使用该模式的库为Enumerable模块和Comparable模块了。
+ Observable(观察者)模式
 - 观察者模式是一种避免高度依赖性的手段。
 - Ruby为实现此模式提供了observer的库。
+ Strategy(策略)模式
>策略模式:定义算法的集合，将各算法封装，使得他们能够交换。利用Strategy模式，算法和利用这些算法的客户程序可以分别独立进行修改而不互相影响。
 - Ruby库中cgi/session库使用了此模式。

####***给定一个时间点，希望得到其他时间点***
使用date库方法即可，如：
```
require 'date'
(Date.new(1776, 7, 2)..Date.new(1776, 7, 4)).each { |x| puts x } 
```

####***求一个数是否为素数***
```
def is_prime?
  return false if self <= 1
  2.upto(Math.sqrt(self).to_i) do |x|
    return false if self % x == 0
  end
  true
end
```
或者使用Prime类方法prime?(n)。