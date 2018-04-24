---
layout: post
title: 'C# 設計模式 - 觀察者模式'
author: 'James Peng'
tags: ['Design Pattern']
---

- 棒球不知道 球員A 和 球員B
- 球員A 和 球員B 都在觀察著球
- 球速低於自己的接球能力 就接住球


~~~csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace WindowsFormsApplication1
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void button1_Click(object sender, EventArgs e)
        {
            Ball ball = new Ball();
            PersonA a = new PersonA(ball);
            PersonB b = new PersonB(ball);
            ball.BallInPlay(40);
        }
    }

    public class BallEventArgs : EventArgs
    {
        public int Speed { get; set; }
    }

    public class Ball
    {
        public event Action<object, BallEventArgs> HitBall;
        public void BallInPlay(int speed)
        {
            if (HitBall != null)
            {
                HitBall(this, new BallEventArgs { Speed = speed });// rise event
            }
        }
    }

    public class PersonA
    {
        public PersonA(Ball ball)
        {
            ball.HitBall += Watch; //binding
        }

        public void Watch(object sender, BallEventArgs e)
        {
            if (e.Speed < 50)
                MessageBox.Show("A: I want to catch ball");
            else
                MessageBox.Show("A: Bye bye");
        }
    }

    public class PersonB
    {
        public PersonB(Ball ball)
        {
            ball.HitBall += Catch;
        }

        public void Catch(object sender, BallEventArgs e)
        {
            if (e.Speed < 90)
                MessageBox.Show("B: I want to catch ball");
            else
                MessageBox.Show("B: Bye bye");
            // update speed into UI form
        }
    }
}

~~~



