---
layout: post
title: '設計模式 - 單一職責原則 Single Responsibility Principle'
author: 'James Peng'
tags: ['Design Pattern']
---

![](..\images\2013-02-07-DesignPattern_SRP\NsNX0Ie.jpg)


## 書摘 ##

單一職責原則 Single Responsibility Principle ，簡稱 SRP。 

原文定義：

    There should never be more than one reason for a class to change. 

翻譯：

    應該有且僅有一個原因，引起class的變化。 


最佳實踐：

 - 建議interface一定要做到單一職責，class的設計盡量做到只有一個原因引起變化。


----------


## 舉例 ##


|         | 餐刀     |   餐叉   |   湯匙 |  筷子   |
|:--------|:-------:|:-------:|:-------:|--------:|
| 國家  | 西方  | 西方  | 西方  | 東方  |
|----
| 學習成本 | 容易  | 容易  | 容易  | 困難 |
|----
| 複雜性 | 低  | 低  | 低  | 高 |
|----
| 職責 | 切牛排  | 固定牛排 | 喝湯  | 夾菜、扒飯、叉貢丸、切肉。 |
|----
| 變化因素 | 食物{x}  | 食物{x}  | 食物{x}  | 食物{x}, 用法{y} |
|----
| 舉例 (new 出的Instance) | 切牛排的餐刀, 切豬排的餐刀, 切雞排的餐刀, 切貢丸的餐刀  | 叉牛排的餐叉, 叉豬排的餐叉, 叉雞排的餐叉, 叉貢丸的餐叉  | 挖湯的湯匙, 挖布丁的湯匙, 挖果凍的湯匙, 挖炒飯的湯匙  | 夾菜的筷子, 挖飯的筷子, 叉貢丸的筷子, 切肉排的筷子 |
|----
| class的變化 | 切{x}的餐刀  | 叉{x}的餐叉  | 挖{x}的湯匙  | 	{y}{x}的筷子, {y}{x}的筷子, {y}{x}的筷子 |
|=====
| 職責數量	 | 單一職責  | 單一職責  | 單一職責  | 多重職責 
{: rules="groups"}


~~~csharp

    public class Knife
    {
        public void cut(string foodSolid)
        {
            MessageBox.Show("切" + foodSolid + "的餐刀");
        }
    }

    public class Fork
    {
        public void cross(string foodSolid)
        {
            MessageBox.Show("叉" + foodSolid + "的餐叉");
        }
    }

    public class Spoon
    {
        public void dig(string foodSolid)
        {
            MessageBox.Show("挖" + foodSolid + "的湯匙");
        }

        public void dig(string foodLiquid)
        {
            MessageBox.Show("挖" + foodLiquid + "的湯匙");
        }
    }

    public class Chopsticks
    {
        public void press(string foodSolid)
        {
            MessageBox.Show("夾" + foodSolid + "的筷子");
        }
        public void dig(string foodSolid)
        {
            MessageBox.Show("挖" + foodSolid + "的湯匙");
        }
        public void cross(string foodSolid)
        {
            MessageBox.Show("叉" + foodSolid + "的筷子");
        }
        public void cut(string foodSolid)
        {
            MessageBox.Show("切" + foodSolid + "的筷子");
        }
    }
~~~

----------

## 讀後感 ##

職責應該單一，並且簡單得讓我不需要深入思考。 

東方人：「吃個飯，洋鬼子拿要一堆餐具，我們拿雙筷子就好了。」（吃飯簡單，但餐具使用變複雜） 

西方人：「吃個飯，黃猴子把餐具搞那麼複雜，我們餐具都不用學。」（餐具簡單，但西餐規矩變多） 

原則是死的，人是活的。還是要自行衡量。（真玄）  

----------

參考：

- 設計模式之禪, 作者：秦小波 , ISBN：9789862760062 

