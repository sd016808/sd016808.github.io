---
layout: post
title: 'Algorithm 實作：河內塔 Towers of Hanoi'
author: 'James Peng'
tags: ['Algorithm']
---

![](..\images\2007-10-12-Algorithm_Towers_of_Hanoi\PR4OFLO.jpg)

原名：Towers of Hanoi

俗稱：河內塔、漢諾塔

河內塔源自於泰國,河內為越戰時北越的首都，即「胡志明市」。

由法國人M.Claus(Lucas)於1883年從泰國帶至法國


    據說創世紀時有一座波羅教塔，
    由三支鑽石棒支撐，神在第一根棒上放置64個「由上至下依由小至大排列的金盤」，
    並命令僧侶將所有的金盤從「第一根鑽石棒」移至「第三根鑽石棒」
    且搬運過程中遵守「大盤子在小盤子之下」的原則，
    每日僅搬一個盤子，當盤子全數搬運完時，此塔將自爆，萬物都將至極樂世界。


----------


## 規則： ##

1. 大盤子在小盤子之下
2. 一次只能搬一個盤子


----------

## 解法： ##

有三個柱子 A柱 B柱 C柱

情況1. 若是 只有一個盤子

- A柱 => C柱 一次搬完。


情況2. 若是 有兩個盤子

- A柱 => B柱  最小的盤子放在B柱暫存。
- A柱 => C柱  把較大的放到C柱。
- B柱 => C柱  完成。

若是 有三個盤子：

1. 把第三個以下先不看, 一次處理兩個盤子, 如「情況2」只是目標是B柱、暫存是C柱：A => C, A => B, C => B
2. 再來，就剩下一個剛剛不看的大盤子 「情況1」 A柱 => C柱
3. 再一次 「情況2」 目標是C柱、暫存是A柱 , 如 B=>A, B=>C , A=>C  完成了

速記公式：n個盤子，移動次數為 2^n - 1

時間複雜度 Time Complexity：T(n)=2^n - 1

 2^64 -1 每秒鐘搬一個盤子，要約5850億年左右。 


----------

## Pseudo code ##

~~~php
Procedure HANOI(n, A, B, C) [  //n是盤子數量,A=來源,B=暫存,C=目的
     IF(n == 1) [
         PRINT("Move sheet " n " from " A " to " C);
     ]  //若盤子數量為1，直接印出「 Move sheet  1  from  A  to  C 」
     ELSE [
         HANOI(n-1, A, C, B); // (把除了最大的盤子以上的小盤子數量,  來源A , 暫存C ,目標 B)  整跎放在B
         PRINT("Move sheet " n " from " A " to " C);  //把最大盤子 放到 C
         HANOI(n-1, B, A, C); //把整跎從 B 放到 C
     ] // 執行三步驟
] 
~~~


----------

## Java code ##

~~~java
import java.io.*;

public class Hanoi {
    public static void main(String args[]) throws IOException {
        int n;
        BufferedReader buf;
        buf = new BufferedReader(new InputStreamReader(System.in));

        System.out.print("請輸入盤數：");
        n = Integer.parseInt(buf.readLine());

        Hanoi hanoi = new Hanoi();
        hanoi.move(n, 'A', 'B', 'C');
    }

    public void move(int n, char a, char b, char c) {
        if(n == 1)
            System.out.println("盤"  + n +  "由"  + a +  "移至"  + c);
        else {
            move(n - 1, a, c, b);
            System.out.println("盤"  + n +  "由"  + a +  "移至"  + c);
            move(n - 1, b, a, c);
        }
    }
}
~~~

![](..\images\2007-10-12-Algorithm_Towers_of_Hanoi\MRkCixu.png)

![](..\images\2007-10-12-Algorithm_Towers_of_Hanoi\uyzYBqe.png)

----------


## C# code ##

~~~csharp
using System;
using System.Text;

public class Hanoi {
    public static void Main(String[] args) {
        int n;
        string buf;

        Console.Write("請輸入盤數：");
 	   buf = Console.ReadLine();

        n = int.Parse(buf);

        Hanoi hanoi = new Hanoi();
        hanoi.move(n, 'A', 'B', 'C');
    }

    public void move(int n, char a, char b, char c) {
        if(n == 1)
            Console.Write("盤"  + n +  "由"  + a +  "移至"  + c +"\n");
        else {
            move(n - 1, a, c, b);
            Console.Write("盤"  + n +  "由"  + a +  "移至"  + c +"\n");
            move(n - 1, b, a, c);
        }
    }
}
~~~

![](..\images\2007-10-12-Algorithm_Towers_of_Hanoi\hpLxrfA.png)

![](..\images\2007-10-12-Algorithm_Towers_of_Hanoi\LFXhbdo.png)



