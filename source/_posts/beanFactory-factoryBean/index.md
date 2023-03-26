---
title: 'BeanFactory FactoryBean'
description:
date: 2021-11-17T15:14:12+08:00
image:
math:
license:
categories:
  - spring
tags:
  - spring
hidden: false
comments: false
draft: false
---

BeanFactory 和 FactoryBean 区别

<!--more-->

1. BeanFactory 定义了 Spring IOC 容器的的基本形式，并提供 IOC 容器应该遵守的基本接口主要抽象类：

   1. AbstractBeanFactory：抽象 Bean 工厂，绝大部分的实现类，都是继承于他
   2. DefaultListableBeanFactory:Spring 默认的工厂类
   3. XmlBeanFactory：前期使用 XML 配置用的比较多的时候用的 Bean 工厂
   4. AbstractXmlApplicationContext:抽象应用容器上下文对象
   5. ClassPathXmlApplicationContext:XML 解析上下文对象，用户创建 Bean 对象

2. FactoryBean 是一个工厂接口 用户可以通过实现该接口定制实例化 bean 逻辑

**区别**

1. BeanFactory:负责生产和管理 Bean 的一个工厂接口，提供一个 Spring Ioc 容器规范,
2. FactoryBean: 一种 Bean 创建的一种方式，对 Bean 的一种扩展。对于复杂的 Bean 对象初始化创建使用其可
   封装对象的创建细节。
