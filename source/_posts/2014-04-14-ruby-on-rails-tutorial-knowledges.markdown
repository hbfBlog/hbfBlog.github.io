---
layout: post
title: "ROR教程知识积累"
date: 2014-04-14 07:26:03 +0800
comments: true
categories: Agile Web Development with Rails4
keywords: Agile Web Development with Rails4
tags: ROR
---
记录在ROR教程学习过程中，遇到的一些问题以及难理解的知识点。
<!--more-->
####***使用AJAX技术***
描述对`button_to`添加AJAX的基本步骤。  
1. 为button添加remote参数：  
    `<%= button_to 'Add to Cart', line_items_path(product_id: product), remote: true %>`
2. 在相应的action动作中添加脚本响应：  
```
  respond_to do |format|
    format.js
  end
```
3.上一步的设置，Rails将会自动寻找一个action.js.erb文件，此步需要新建一个该文件（与相应的view文件在同一个目录中）。  
        例如创建文件app/views/line_items/create.js.erb  
        使用JQuery编写该js脚本文件：`$('#cart').html("<%= escape_javascript render(@cart) %>");`  

####***使应用变得国际化***  
1. 新建文件app/config/initializers/i18n.rb,设定语言种类以及默认语言。  
```
I18n.default_locale = :en
LANGUAGES = [
    ['English', 'en'],  
    ['简体中文', 'zh-CN']
]
```
2. 创建'/en/controller/action'形式路由  
```
scope '(:locale)' do
    root 'store#index', as: 'store', via: :all
end
```
3. 在`application_controller.rb`中设置全局的语言选择  
```
before_action :set_i18n_locale_from_params  
...
protected
  def set_i18n_locale_from_params
    if params[:locale]
     if I18n.available_locales.map(&:to_s).include?(params[:locale])
        I18n.locale = params[:locale]
     else
        flash.now[:notice] = "#{params[:locale]} translation not available"
        logger.error flash.now[:notice]
     end
    end

    def default_url_options
      { locale: I18n.locale }
    end
  end
```
4. 设置`app/views/layouts/application.html.erb`  
`<li><a href=""><%= t('.news') %></a></li>`
5. 编写相应的国际化输出  
编辑app/config/locales/en.yml
```
en:  
  layouts:
    application:
       news: "News"
```
新建并编辑app/config/locales/zh-CN.yml
```
zh-CN:
  layouts:
    application:
       news: "新闻"
```
6. 添加语言选择器  
在application.html.erb中添加下拉按：  
```
<div id="banner">  
      <%= form_tag store_path, class: 'locale' do %>  
           <%= select_tag 'set_locale', options_for_select(LANGUAGES, I18n.locale.to_s),onchange: 'this.form.submit()' %>  
           <%= submit_tag 'submit' %>  
           <%= javascript_tag "$('.locale input').hide()" %>  
      <% end %>  
</div>  
```
在控制器中处理参数:  
```
def index  
      if params[:set_locale]  
        redirect_to store_url(locale: params[:set_locale])  
      else  
        @products = Product.order(:title)  
      end  
end  
```