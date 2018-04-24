---
layout: post
title: 'Setup Rails for Windows'
author: 'James Peng'
tags: ['Rails']
---

## 一鍵安裝 Rails ##

對於初學者，在Windwos上安裝rails最簡單的方式是RailsInstaller安裝包。

RailsInstaller是一鍵安裝包，能夠幫助你盡快上手，快速安裝好開發環境。

1. 下載最新的 [RailsInstaller](http://railsinstaller.org/en) 並執行。使用預設選項，一直按下一步。

![](..\images\2015-02-13-setup_rails_for_windows\Sad9e00.png)


railsinstaller安裝步驟

![](..\images\2015-02-13-setup_rails_for_windows\j1aCju2.png)


同意安裝協議，進入下一步

![](..\images\2015-02-13-setup_rails_for_windows\SDFLzd9.png)

這一步設置文件安裝的位置，推薦使用英文或者拼音，字母間一定不要帶空格，方便以後通過 終端機（Windows 叫做命令提示字元）的方式進行操作，點擊install進行安裝。

![](..\images\2015-02-13-setup_rails_for_windows\kXwefL5.png)

等待幾分鐘

![](..\images\2015-02-13-setup_rails_for_windows\kQkmcQn.png)

點擊finish完成安裝。

![](..\images\2015-02-13-setup_rails_for_windows\I9ZwQOM.png)

點擊完成後，會彈出設置使用者名稱，如下圖。

![](..\images\2015-02-13-setup_rails_for_windows\QZzQgJr.png)

輸入email

![](..\images\2015-02-13-setup_rails_for_windows\paKRVzl.png)

完成後，會給一組ssh key把它複製起來，建立一個txt文件，把它保存下來，留著以後使用。

![](..\images\2015-02-13-setup_rails_for_windows\47LCEZx.png)


查看Rails版本

```
rails -v
```

查看Ruby版本

```
ruby -v
```

查看gem版本

```
gem -v
```

![](..\images\2015-02-13-setup_rails_for_windows\qQo3eVX.png)

看到這裡我們的windows已經快速的安裝了Ruby on Rails

-------------------

## 更新 Rails 版本 ##

但是不要高興的太早 Rails 版本不是最新的

我們可以在 終端機（Windows 叫做命令提示字元）輸入下面的命令來更新 Rails 版本。

```
gem update rails --no-ri --no-rdoc
```

![](..\images\2015-02-13-setup_rails_for_windows\OhZUMot.png)


這時候出現一個錯誤訊息

> C:\Sites>gem update rails --no-ri --no-rdoc
> Updating installed gems
> ERROR:  While executing gem ... (Gem::RemoteFetcher::FetchError)
>     SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B: certif
> icate verify failed (https://api.rubygems.org/specs.4.8.gz)

這個[SSL error](https://gist.github.com/luislavena/f064211759ee0f806c88)的問題，要更新RubyGems的版本

- gem 版本 1.8.x: 下載 [1.8.30](https://github.com/rubygems/rubygems/releases/tag/v1.8.30)
- gem 版本 2.0.x: 下載 [2.0.15](https://github.com/rubygems/rubygems/releases/tag/v2.0.15)
- gem 版本 2.2.x: 下載 [2.2.3](https://github.com/rubygems/rubygems/releases/tag/v2.2.3)

這裡剛剛查過gem版本是 2.2.2，下載 rubygems-update-2.2.3.gem

![](..\images\2015-02-13-setup_rails_for_windows\c9ZX2z2.png)

下載到工作目錄 C:\Sites，安裝 RubyGems

```
gem install --local rubygems-update-2.2.3.gem
update_rubygems --no-ri --no-rdoc
```
![](..\images\2015-02-13-setup_rails_for_windows\Wgae0R2.png)

查看版本
```
gem -v
```
應該會更新到 2.2.3
![](..\images\2015-02-13-setup_rails_for_windows\AVZBfCa.png)


這時候執行 update rails 就正常更新了
```
gem update rails --no-ri --no-rdoc
```
![](..\images\2015-02-13-setup_rails_for_windows\kd9TqU4.png)

--------------------------

## 確認 Rails 安裝成功 ##

確認 Rails 有沒有安裝成功，試試在終端機（Windows 叫做命令提示字元）輸入下面的命令：

建立一個嶄新的 Rails App，名字叫做 RailsDemo
```
rails new RailsDemo
```

![](..\images\2015-02-13-setup_rails_for_windows\2nbuQGY.png)

命令切換到 Rails APP 資料夾：

```
cd RailsDemo
```

可以用下面這個命令來啟動 Rails 伺服器：

```
ruby bin\rails server
```

![](..\images\2015-02-13-setup_rails_for_windows\ZByZBtQ.png)

在瀏覽器打開 [http://localhost:3000/](http://localhost:3000/)。應該會看到 "Welcome aboard" 的頁面，代表 APP 產生成功了！

![](..\images\2015-02-13-setup_rails_for_windows\HurwwNo.png)

在終端機（Windows 叫做命令提示字元）按 CTRL-C 來離開伺服器。

----------------------------

## 安裝編輯環境 ##

最後一步 會需要文字編輯器來編輯程式碼。

我們推薦使用現在最潮的editor [Sublime Text 編輯器](http://www.sublimetext.com/2)。

![](..\images\2015-02-13-setup_rails_for_windows\kljCol3.png)

![](..\images\2015-02-13-setup_rails_for_windows\7IbLrMh.png)

[附上低調的東東](http://www.yinqisen.cn/blog-246.html)

    ----- BEGIN LICENSE -----
    Andrew Weber
    Single User License
    EA7E-855605
    813A03DD 5E4AD9E6 6C0EEB94 BC99798F
    942194A6 02396E98 E62C9979 4BB979FE
    91424C9D A45400BF F6747D88 2FB88078
    90F5CC94 1CDC92DC 8457107A F151657B
    1D22E383 A997F016 42397640 33F41CFC
    E1D0AE85 A0BBD039 0E9C8D55 E1B89D5D
    5CDB7036 E56DE1C0 EFCC0840 650CD3A6
    B98FC99C 8FAC73EE D2B95564 DF450523
    ------ END LICENSE ------

聽說學習Ruby on Rails完全不用買新的mac

用MAC寫Ruby on Rails最常用的三個工具是


1. 編輯器：Sublime
2. 終端機：Terminal（Windows 叫做命令提示字元）
3. 瀏覽器：Chrome

這三個程式在Ubuntu上也可以灌(Terminal不用灌)，可以說寫程式的體驗一模一樣

連Ubuntu都沒得灌的情況下，現在我們在 Windows 也能先行體驗 Ruby on Rails 了！
