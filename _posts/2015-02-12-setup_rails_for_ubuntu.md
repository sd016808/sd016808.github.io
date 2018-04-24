---
layout: post
title: 'Setup Rails for Ubuntu'
author: 'James Peng'
tags: ['Rails']
---


# setup


首先進行Linux更新：
```
sudo apt-get update
sudo apt-get upgrade
```



安裝必要的套件：

~~~text
sudo apt-get install build-essential bison openssl libreadline6 libreadline6-dev curl git-core zlib1g zlib1g-dev libssl-dev libyaml-dev libsqlite3-0 libsqlite3-dev sqlite3 libxml2-dev libxslt-dev autoconf libc6-dev
~~~


接著下載Ruby原始碼編譯，請參考Ruby官網下載最新2.1.5版本：

~~~text
 wget http://cache.ruby-lang.org/pub/ruby/2.1/ruby-2.1.5.tar.gz
 tar xvfz ruby-2.1.5.tar.gz
 cd ruby-2.1.5/
 ./configure
 make
 sudo make install
~~~

安裝完成之後輸入以下指令可以看到安裝的版本：

~~~text
 git --version
 ruby -v
~~~

--------------------------

#### 安裝Ruby on Rails

```
sudo gem install rails
```

------------------------

遞迴修改目前資料夾下的所有資料夾

```
find . -type d -exec chmod 777 {} \;
```

遞迴修改目前資料夾下的所有檔案 為777

```
find . -type f -exec chmod 777 {} \;
```



https://ihower.tw/rails4/installation.html




