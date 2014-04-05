---
layout: post
title: "如何使用Octopress框架建博客"
date: 2014-04-05 12:12:02 +0800
comments: true
categories: [octopress, rake(zsh)]
---
##第一步：安装Octopress的环境：  
  1.检查Ruby版本(>=1.9.3): 
    ruby -v =>ruby 2.0.0p247  
  2.克隆一个Octopress仓库到本地： 
    git clone https://github.com/BaifanHe/octopress.git octopress 
    cd octopress 
  3.安装依赖项 
    gem install bundler 
    bundle install 
    rake install 
##第二步：部署到Github Pages：  
  1.在Github上创建一个`hbfBlog`小组。  
  2.在该小组中创建一个仓库（命名与组名一致）：`hbfBlog.github.io`  
  3.运行命令`rake setup_github_pages`: 
    由于已经安装有最新版的bundler，用该命令无法直接使用Octopress框架依赖的低版本，使用如下格式：
    `bundle exec rake setup_github_pages`  
  4.命令提示请输入Github仓库地址的时候，输入： 
    https://github.com/hbfBlog/hbfBlog.github.io.git 
  5.生成博客页面框架：`rake generate`生成博客页面存放在`public`目录下。   
  6.部署博客页面框架：`rake deploy`将`public`目录中的内容存放入`_deploy`目录中。 
##第三步：同步到Github: 
    git add .  
    git commit -am "create myself blog"   
    git push origin source  
##第四步：新建博客:
    rake new_post["how to using octoprocess"]  
    => zsh: no matches found: new_post[how to using octopress]  
  由于使用的是`zsh shell`，因此运行命令的时候需要做反义字符处理: 
    rake new_post\["how to using octoprocess"\] 
    =>Creating new page: source/_post/2014-04-05-how-to-using-octopress.markdown  
  编辑markdown文件，创作文章。
##第五步：新建关于页面:  
    bundle exec rake new_page\["about"\]                
    mkdir -p source/about  
    Creating new page: source/about/index.markdown  
  添加关于链接：
    在source\_includes\custom\navigation.html中添加以下内容:
    <li><a href="{{ root_url }}/about">About</a></li>
##第六步：查看变更和预览:  
    bundle exec rake generate  
    bundle exec rake watch  
    bundle exec rake preview  
  使用浏览器查看：`http://localhost:4000`，即可进行预览。
##第七步: 实施博客:  
    bundle exec rake deploy
##第八步: 提交到Github:  
    git add .  
    git status  
    git commit -am "create a new post and about page"  
    git push origin source  
##第九步：访问
  http://hbfblog.github.io