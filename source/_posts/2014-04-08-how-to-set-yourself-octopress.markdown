---
layout: post
title: "如何配置Octopress"
date: 2014-04-08 21:41:06 +0800
comments: true
categories: octopress
keywords: [octopress,octopress setting]
tags: octopress
---
记录一些对框架的基本配置以及侧边栏小功能的添加过程和遇到的问题。
<!--more-->
####执行`rake install`命令后生成的文件夹有：
1. source：对.themes\classic\source目录的一份拷贝并自动创建_posts目录。
2. sass：对.themes\class\sass的一份拷贝。
3. public：用于存储`rake generate`命令对source目录下的资源文件进行编译后的内容，并在执行`rake deploy`命令后将其内容推送到服务器上。此文件夹内容不需要进行手动修改。

####对"_config.yml"的基本设置：
        url: http://hbfBlog.github.io     #博客网址
      title: DevinHe Blog           #博客标题
      subtitle: 花不可以无蝶，山不可以无泉，人不可以无癖。       #博客副标题
      author: DevinHe                       #博客的作者
      simple_search: https://www.google.com/search        #搜索引擎
      description: 个人学习网站                   #网站的描述

####对博文的基本设置：
    ---
    layout: post
    title: "how to set yourself octopress"      #文章的标题
    date: 2014-04-08 21:41:06 +0800     #文章的发布时间
    comments: true                #是否允许评论
    categories: octopress           #文章类别
    keywords: octopress setting         #文章关键字
    tags: octopress               #文章标签
    description: "config.yml, index, nav"
    ---
####对首页的设置：
    ---
    layout: default
    description: "何白帆的个人博客" 
    keywords: Ruby,Rails,Web
    ---
####导航栏的设置（source\_includes\custom\navigation.html）：
  首先运行命令`rake new_page['about']`创建一个关于页面。 
  修改导航页面：
    <ul class="main-navigation"> 
      <li><a href="{{ root_url }}/">首页</a></li> 
      <li><a href="{{ root_url }}/blog/archives">归档</a></li> 
      <li><a href="{{ root_url }}/about">关于</a></li> 
    </ul>
####添加侧边栏文章分类
1. 在plugins目录中创建category_list_tag.rb文件，内容如下：
```
module Jekyll   
      class CategoryListTag < Liquid::Tag   
        def render(context)   
          html = ""   
          categories = context.registers[:site].categories.keys   
          categories.sort.each do |category|   
            posts_in_category = context.registers[:site].categories[category].size   
            category_dir = context.registers[:site].config['category_dir']   
            category_url = File.join(category_dir, category.gsub(/_|\P{Word}/, '-').gsub(/-{2,}/, '-').downcase)   
            html << "<li class='category'><a href='/#{category_url}/'>#{category} (#{posts_in_category})</a></li>\n"   
          end   
          html   
        end   
      end   
    end  
    Liquid::Template.register_tag('category_list', Jekyll::CategoryListTag)  
``` 
2. 添加source/_includes/asides/category_list.html文件，内容如下：
```
<section> 
  <h1>Categories</h1> 
  <ul id="categories"> 
    {% category_list %} 
  </ul> 
</section>
```
3. 修改_config.yml文件  
`default_asides: [asides/category_list.html, asides/recent_posts.html]`

####添加评论功能（使用Disqus第三方评论系统）：
1. 首先注册一个[Disqus](http://www.disqus.com/)账号。
2. 修改`_config.yml`文件
        # Disqus Comments 
        disqus_short_name: xxxx   # xxxx为添加站点信息时的Site Shortname，即unique Disqus URL。
        disqus_show_comment_count: true

####使文章以摘要形式展示
    在显示内容后面添加<!—more—>。

####添加tags：  
1. [octopress-tag-pages](https://github.com/robbyedwards/octopress-tag-pages)  
    * 复制tag_generator.rb到/plugins目录;  
    * 复制tag_index.html到/source/_layouts目录;  
    * 复制tag_feed.xml到source/_includes/custom目录.  
2. [octopress-tag-cloud](https://github.com/robbyedwards/octopress-tag-cloud)
    * 复制tag_cloud.rb到/plugins目录。
3. bug:当对所有标签只出现一次的情况编译时，会报错。  
    [Ref](https://github.com/robbyedwards/octopress-tag-cloud/issues/1)  
    错误信息：Liquid Exception: comparison of Array with Array failed in page  
    修改plugins/tag_cloud.rb文件中如下内容：
          # get the minimum, and maximum tag count
          min, max = count.map(&:last).minmax
        
          # map: [[tag name, tag count]] -> [[tag name, tag weight]]
          weighted = count.map do |name, count|
            # logarithmic distribution
            if min == max
              weight = 1
            else
              weight = (Math.log(count) - Math.log(min))/(Math.log(max) - Math.log(min))
            end
            [name, weight]
          end 