---
title: "如何讲明白技术"
date: 2019-05-08T22:22:54+08:00
draft: false
author: "kenwood"
isCJKLanguage: true
tags: ["随想"]
---





在面试的时候，技术问题所占的比重非常大，如何讲明白技术，决定了面试的成败。从某种程度，也体现你对技术的了解程度，所以如何讲明白技术。如何在学习的时候，对技术知识点进行加工内化，以下分析角度来源极客时间专栏：面试现场，我会尝试从这个角度去写的我技术文章

![1557325773291](/home/kenwood/Projects/my-website/static/images/1557325773291.png)

## 外部应用维度

### 问题

首先考虑的是要解决什么问题，这是技术产生的原因。Java 多线程的产生，是因为要并发，并发使得程序的多种功能响应更快，用户体验更好。问题这层就是回答干什么用

### 技术规范

怎么用这门技术，技术使用说明书

### 最佳实践

把技术应用到不同的场景时，发发现同样的使用方法，会有不同的效果。该技术有不同的适应面。从而你可能踩了很多坑，从中总结出最佳实践

### 市场应用趋势

随着技术生态的发展，和应用问题的变迁，技术的应用场景和流行趋势会受到影响。对于java，从低并发逐渐发展到高并发。谁在用，用在哪

## 设计维度

应用维度是从外部看技术的应用，那么从内部能看到技术的哪些层面？

### 目标

为了解决用户的问题，技术本身要达成什么目标。比如，java多线程在优先级调度、锁、信息同步等方面达成怎样的目标，才能更好地实现并非

### 实现原理

为了达到设计目标，该技术采用了什么原理和机制。java多线程的实现原理包括内核线程，使用用户态线程加轻量级进程混合等。实现原理层回答怎么做到的问题。把实现原理弄懂，并且讲清楚，是技术人员的基本

### 优劣局限

每种技术实现，都有其局限性，在某些条件下能最大化的发挥效能，缺少某些条件则暴露出其缺陷。比如在java多线程编程中，采用共享内存的方式，锁的开销比较大，程序员编程难度大。 优劣局限层回答“做得怎么样的问题。对技术优劣局限的把握，更有利于应用时总结最佳实践，是分析各种坑的基础

### 演进趋势

技术是在迭代改进和不断淘汰的。了解技术的前生后世，分清技术不变的本质，和变化的脉络，以及与其他技术的共生关系，能体现你对技术发展趋势的关注和思考。