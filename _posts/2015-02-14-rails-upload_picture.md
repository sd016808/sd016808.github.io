---
layout: post
title: 'Rails 圖片上傳'
author: 'James Peng'
tags: ['Rails']
---


我們需要安裝一點軟體（Gem），讓我們可以在 Rails 裡上傳檔案。


~~~text
    gem install 'carrierwave'
~~~

~~~text
 C:\Sites\idea>gem install 'carrierwave'
 Fetching: carrierwave-0.10.0.gem (100%)
 Successfully installed carrierwave-0.10.0
 Parsing documentation for carrierwave-0.10.0
 Installing ri documentation for carrierwave-0.10.0
 Done installing documentation for carrierwave after 8 seconds
 1 gem installed
~~~

![](..\images\2015-02-14-rails-upload_picture\WcRIfcZ.png)


在終端裡，執行：

~~~text
    bundle
~~~

現在我們可以來產生處理上傳的相關程式。在終端裡，執行：

~~~text
    rails generate uploader Picture
~~~


打開 app/models/idea.rb 並在這行下面


~~~ruby
    class Idea < ActiveRecord::Base
~~~



加入

~~~ruby
    mount_uploader :picture, PictureUploader
~~~


打開 app/views/ideas/_form.html.erb 並將這行

~~~ruby
    <%= f.text_field :picture %>
~~~

改成

~~~ruby
    <%= f.file_field :picture %>
~~~

當上傳一張圖片，它顯示的是圖片的路徑，所以讓我們讓圖片顯示吧。

打開 app/views/ideas/show.html.erb 並將這兩行

~~~ruby
    <strong>Picture:</strong>
    <%= @idea.picture %>
~~~

改成

~~~ruby
    <%= image_tag(@idea.picture_url, width: 600) if @idea.picture.present? %>
~~~

現在重新整理瀏覽器。
