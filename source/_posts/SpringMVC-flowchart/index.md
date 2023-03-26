---
title: 'SpringMVC Flowchart'
description:
date: 2021-11-03T17:21:58+08:00
image:
math:
license:
categories:
  - springmvc
tags:
  - spring
hidden: false
comments: false
draft: false
---

# SpringMVC

## 简单原理图：

![](https://s1.ax1x.com/2020/07/04/Nxg2RI.png#height=369&id=SvwHh&originHeight=369&originWidth=804&originalType=binary&status=done&style=none&width=804)

## 详细原理图：

​[![gRvB6I.png](https://z3.ax1x.com/2021/05/17/gRvB6I.png)](https://imgtu.com/i/gRvB6I)

## 流程:

1. 浏览器发送请求，到 DispatcherServlet（前端控制器）
2. DispatcherServlet 根据请求信息调用 HandlerMapping（处理器映射器），解析对应 Handler
3. 解析对应 Handler（Controller 控制器）后，开始由 HandlerAdapter 适配器处理。
4. HandlerAdapter 会根据 Handler 来调用处理器处理请求，并处理相应的业务逻辑。
5. 处理完业务后，会返回 ModelAndView 对象
6. ViewResolver 根据逻辑 view 查找到实际 view
7. DispaterServlet 返回 Model 传给 View
8. 把 View 传回浏览器

## 各处理器或控制器作用

1. 前端控制器 DispatcherServlet（不需要程序员开发）,由框架提供，在 web.xml 中配置。作用：接收请求，
   响应结果，相当于转发器，中央处理器。

2. 处理器映射器 HandlerMapping(不需要程序员开发),由框架提供。

作用：根据请求的 url 查找 Handler(处理器/Controller)，可以通过 XML 和注解方式来映射。

3. 处理器适配器 HandlerAdapter(不需要程序员开发),由框架提供。

作用：按照特定规则（HandlerAdapter 要求的规则）去执行 Handler。

4. 处理器 Handler(也称之为 Controller，需要工程师开发)

注意：编写 Handler 时按照 HandlerAdapter 的要求去做，这样适配器才可以去正确执行 Handler。作用：接受
用户请求信息，调用业务方法处理请求，也称之为后端控制器。

5. 视图解析器 ViewResolver(不需要程序员开发),由框架提供

作用：进行视图解析，把逻辑视图名解析成真正的物理视图。 SpringMVC 框架支持多种 View 视图技术，包括
：jstlView、freemarkerView、pdfView 等。

6. 视图 View(需要工程师开发)

作用：把数据展现给用户的页面 View 是一个接口，实现类支持不同的 View 技术（jsp、freemarker、pdf 等）
