---
title: Instruments的使用
date: 2016-11-18 23:31:30
description: Instruments是一个强大而灵活的性能分析和测试工具，它是Xcode开发工具集的一部分，它被设计用于帮助分析OS X和iOS的应用、进程与设备，以便更好的理解和优化它们的行为和表现。结合**Instruments**从你的app开发工作刚刚开始，可以节省你的时间，帮助你在开发周期的早期发现问题
categories:
- 调试相关
tags:
- instruments
---

## Instruments的介绍

>**Instruments**是一个强大而灵活的性能分析和测试工具，它是Xcode开发工具集的一部分，它被设计用于帮助分析OS X和iOS的应用、进程与设备，以便更好的理解和优化它们的行为和表现。结合**Instruments**从你的app开发工作刚刚开始，可以节省你的时间，帮助你在开发周期的早期发现问题。

>在**Instruments**中，可以使用指定的工具，跟踪应用app、进程、设备不同方面的行为，**Instruments**将数据收集在配置文件中，并将详细分析的结果显示出来

## Instruments配置

**Instruments**在默认的情况下是在`Release`模式下编译构建的，这样可能会表现出与DeBug模式完全不同的性能，如果想在`DeBug`模式下分析可以在`Xcode-Product-Scheme-EditScheme`中修改`Build Configguration`为`DeBug`，同时我们也可以修改**instrument**的默认启动

![png1](/assets/images/instrument1.jpg)

## Leaks

点击`Xcode-Product`下的`Profile`，**instruments**将会启动，界面如下

![png1](/assets/images/instrument2.jpg)

这里我们打开`leak`内存泄漏的工具，点击红色按钮开始查找，同时我们对程序进行操作

![png1](/assets/images/instrument3.jpg)

绿色代表没有内存泄漏，红色叉号代表出现了可能有内存泄漏的情况，点击暂停按钮，
点击红色叉号，同时切换到`Leaks`，选择`Cycles&Roots`查看是否由循环引用

![png1](/assets/images/instrument4.jpg)

通过关系图我们可以看到是否发生了循环引用，然后点击`Call Tree`调用栈，点击底部的`Call Tree`勾选`invert Call (反转调用关系树)Tree`与`Hide System Libraies(忽略系统调用)`,可以看到有两个方法中可能存在内存泄漏，双击其中的方法

![png1](/assets/images/instrument5.jpg)

分别跳到以下两个地方

![png1](/assets/images/instrument6.jpg)

![png1](/assets/images/instrument7.jpg)

通过查看，发现第一个方法并没什么问题，而第二个方法存在循环引用，导致内存泄漏，修改其代码即可
需要注意的是`Leaks`会有一些错误的测试结果，比如它经常会程序启动时显示一两个微小的泄露，即使你的代码没有内存泄露问题，所以不要抱有太高的希望，认真核对。

## Time Profiler

耗时很长的操作通常是因为它需要大量CPU资源，或者是它被阻塞，在主线程中，阻塞操作是很严重的问题，因为UI不能更新，要找出阻塞操作，可以使用`Time Profiler`，点击`i`号按钮显示选项，然后选择`Record Wating Theads`，在`Sample Perspective`中选择`All Sample Counts`。这样就能很方便的看出每个线程中（尤其是主线程）哪种操作最耗时。

## Core Animation

`Core Animation`工具显示了程序每秒的帧率，能够帮助调试绘图的问题。

*******

熟练的使用Instruments能够帮助我们更好的分析应用，对程序的优化有很大帮助，是一款难得的利器。
