---
layout: post
title: 'github: Page build failure'
author: 'James Peng'
tags: ['Git']
---

github page 昨天還builde 好好的，完全沒改動的情況下，今天始終Page build failure，

> The page build failed with the following error:
> Page build failed. For more information, see https://help.github.com/articles/troubleshooting-github-pages-build-failures.
> If you have any questions you can contact us by replying to this email.

最後才發現問題在於 highlight 語法。




解決方案：

1.把_config.yml裡的

~~~text
    markdown: kramdown 
~~~

改成

~~~text
    markdown: redcarpet
~~~

2.把post內的.md文章 有用到 highlight 語法

```
{％ highlight cpp ％}
// Code
{％ endhighlight ％}
```

改成

    ~~~cpp
    // Code
    ~~~

搞定！


附註：

- C#的語法：不是 ~~~cs 而是 ~~~csharp
- .yml的語法：不是 ~~~yml 而是 ~~~yaml
- 純文字 ~~~text


參考：

[http://loyc.net/2014/blogging-on-github.html](http://loyc.net/2014/blogging-on-github.html)
