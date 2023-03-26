---
title: 'Spring Life Cycle'
description: 'Spring Life Cycle'
date: 2021-11-03T17:06:47+08:00
image:
math:
license:
hidden: false
comments: false
draft: false
---

## Spring 生命周期

<!--more-->

```bash
Spring容器初始化
=====================================
调用GiraffeService无参构造函数
GiraffeService中利用set方法设置属性值
调用setBeanName:: Bean Name defined in context=giraffeService
调用setBeanClassLoader,ClassLoader Name = sun.misc.Launcher$AppClassLoader
调用setBeanFactory,setBeanFactory:: giraffe bean singleton=true
调用setEnvironment
调用setResourceLoader:: Resource File Name=spring-beans.xml
调用setApplicationEventPublisher
调用setApplicationContext:: Bean Definition Names=[giraffeService, org.springframework.context.annotation.CommonAnnotationBeanPostProcessor#0, com.giraffe.spring.service.GiraffeServicePostProcessor#0]
执行BeanPostProcessor的postProcessBeforeInitialization方法,beanName=giraffeService
调用PostConstruct注解标注的方法
执行InitializingBean接口的afterPropertiesSet方法
执行配置的init-method
执行BeanPostProcessor的postProcessAfterInitialization方法,beanName=giraffeService
Spring容器初始化完毕
=====================================
从容器中获取Bean
giraffe Name=李光洙
=====================================
调用preDestroy注解标注的方法
执行DisposableBean接口的destroy方法
执行配置的destroy-method
Spring容器关闭
```

## 参考文档

[life cycle management of a spring bean](http://www.journaldev.com/2637/spring-bean-life-cycle#comment-35644)
[Spring Bean Life Cycle](http://javabeat.net/life-cycle-management-of-a-spring-bean/)

## Spring Bean 的生命周期

先来看看，Spring 在 Bean 从创建到销毁的生命周期中可能做得事情。

### initialization 和 destroy

有时我们需要在 Bean 属性值 set 好之后和 Bean 销毁之前做一些事情，比如检查 Bean 中某个属性是否被正常的设置好值了。Spring 框架提供了多种方法让我们可以在 Spring Bean 的生命周期中执行 initialization 和 pre-destroy 方法。

**1.实现 InitializingBean 和 DisposableBean 接口**

这两个接口都只包含一个方法。通过实现 InitializingBean 接口的 afterPropertiesSet()方法可以在 Bean 属性值设置好之后做一些操作，实现 DisposableBean 接口的 destroy()方法可以在销毁 Bean 之前做一些操作。

🌰 如下：

```java
public class GiraffeService implements InitializingBean,DisposableBean {
    @Override
    public void afterPropertiesSet() throws Exception {
        System.out.println("执行InitializingBean接口的afterPropertiesSet方法");

    }

    @Override
    public void destroy() throws Exception {
        System.out.println("执行DisposableBean接口的destroy方法");
    }
}
```

这种方法比较简单，但是不建议使用。因为这样会将 Bean 的实现和 Spring 框架耦合在一起。

**2.在 bean 的配置文件中指定 init-method 和 destroy-method 方法**

Spring 允许我们创建自己的 init 方法和 destroy 方法，只要在 Bean 的配置文件中指定 init-method 和 destroy-method 的值就可以在 Bean 初始化时和销毁之前执行一些操作。
🌰 如下：

```java
public class GiraffeService {
    //通过<bean>的destroy-method属性指定的销毁方法
    public void destroyMethod() throws Exception {
        System.out.println("执行配置的destroy-method");
    }

    //通过<bean>的init-method属性指定的初始化方法
    public void initMethod() throws Exception {
        System.out.println("执行配置的init-method");
    }

}
```

配置文件中的配置：

```xml
<bean name="giraffeService" class="com.giraffe.spring.service.GiraffeService" init-method="initMethod" destroy-method="destroyMethod">
</bean>
```

需要注意的是自定义的 init-method 和 post-method 方法可以抛异常但是不能有参数。
这种方式比较推荐，因为可以自己创建方法，无需将 Bean 的实现直接依赖于 spring 的框架。

**3.使用@PostConstruct 和@PreDestroy 注解**

除了 xml 配置的方式，Spring 也支持用`@PostConstruct`和 `@PreDestroy`注解来指定 init 和 destroy 方法。这两个注解均在`javax.annotation`包中。
为了注解可以生效，需要在配置文件中定义`org.springframework.context.annotation.CommonAnnotationBeanPostProcessor`或`context:annotation-config`
🌰 如下：

```java
public class GiraffeService {
    @PostConstruct
    public void initPostConstruct(){
        System.out.println("执行PostConstruct注解标注的方法");
    }

    @PreDestroy
    public void preDestroy(){
        System.out.println("执行preDestroy注解标注的方法");
    }

}

```

配置文件:

```xml
<bean class="org.springframework.context.annotation.CommonAnnotationBeanPostProcessor" />
```

### 实现\*Aware 接口 在 Bean 中使用 Spring 框架的一些对象

有些时候我们需要在 Bean 的初始化中使用 Spring 框架自身的一些对象来执行一些操作，比如获取 ServletContext 的一些参数，获取 ApplicaitionContext 中的 BeanDefinition 的名字，获取 Bean 在容器中的名字等等。为了让 Bean 可以获取到框架自身的一些对象，Spring 提供了一组名为\*Aware 的接口。
这些接口均继承于`org.springframework.beans.factory.Aware`标记接口，并提供一个将由 Bean 实现的 set\*方法,Spring 通过基于 setter 的依赖注入方式使相应的对象可以被 Bean 使用。
网上说，这些接口是利用观察者模式实现的，类似于 servlet listeners，目前还不明白，不过这也不在本文的讨论范围内。
介绍一些重要的 Aware 接口：

- ApplicationContextAware: 获得 ApplicationContext 对象,可以用来获取所有 Bean definition 的名字。
- BeanFactoryAware:获得 BeanFactory 对象，可以用来检测 Bean 的作用域。
- BeanNameAware:获得 Bean 在配置文件中定义的名字。
- ResourceLoaderAware:获得 ResourceLoader 对象，可以获得 classpath 中某个文件。
- ServletContextAware:在一个 MVC 应用中可以获取 ServletContext 对象，可以读取 context 中的参数。
- ServletConfigAware 在一个 MVC 应用中可以获取 ServletConfig 对象，可以读取 config 中的参数。

🌰 如下:

```java
public class GiraffeService implements   ApplicationContextAware,
        ApplicationEventPublisherAware, BeanClassLoaderAware, BeanFactoryAware,
        BeanNameAware, EnvironmentAware, ImportAware, ResourceLoaderAware{
         @Override
    public void setBeanClassLoader(ClassLoader classLoader) {
        System.out.println("执行setBeanClassLoader,ClassLoader Name = " + classLoader.getClass().getName());
    }

    @Override
    public void setBeanFactory(BeanFactory beanFactory) throws BeansException {
        System.out.println("执行setBeanFactory,setBeanFactory:: giraffe bean singleton=" +  beanFactory.isSingleton("giraffeService"));
    }

    @Override
    public void setBeanName(String s) {
        System.out.println("执行setBeanName:: Bean Name defined in context="
                + s);
    }

    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        System.out.println("执行setApplicationContext:: Bean Definition Names="
                + Arrays.toString(applicationContext.getBeanDefinitionNames()));

    }

    @Override
    public void setApplicationEventPublisher(ApplicationEventPublisher applicationEventPublisher) {
        System.out.println("执行setApplicationEventPublisher");
    }

    @Override
    public void setEnvironment(Environment environment) {
        System.out.println("执行setEnvironment");
    }

    @Override
    public void setResourceLoader(ResourceLoader resourceLoader) {

        Resource resource = resourceLoader.getResource("classpath:spring-beans.xml");
        System.out.println("执行setResourceLoader:: Resource File Name="
                + resource.getFilename());

    }

    @Override
    public void setImportMetadata(AnnotationMetadata annotationMetadata) {
        System.out.println("执行setImportMetadata");
    }
}
```

### BeanPostProcessor

上面的\*Aware 接口是针对某个实现这些接口的 Bean 定制初始化的过程，
Spring 同样可以针对容器中的所有 Bean，或者某些 Bean 定制初始化过程，只需提供一个实现 BeanPostProcessor 接口的类即可。 该接口中包含两个方法，postProcessBeforeInitialization 和 postProcessAfterInitialization。 postProcessBeforeInitialization 方法会在容器中的 Bean 初始化之前执行， postProcessAfterInitialization 方法在容器中的 Bean 初始化之后执行。
🌰 如下：

```java
public class CustomerBeanPostProcessor implements BeanPostProcessor {

    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("执行BeanPostProcessor的postProcessBeforeInitialization方法,beanName=" + beanName);
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("执行BeanPostProcessor的postProcessAfterInitialization方法,beanName=" + beanName);
        return bean;
    }


}
```

要将 BeanPostProcessor 的 Bean 像其他 Bean 一样定义在配置文件中

```xml
<bean class="com.giraffe.spring.service.CustomerBeanPostProcessor"/>
```

## 总结

所以。。。结合第一节控制台输出的内容，Spring Bean 的生命周期是这样纸的：

- Bean 容器找到配置文件中 Spring Bean 的定义。
- Bean 容器利用 Java Reflection API 创建一个 Bean 的实例。
- 如果涉及到一些属性值 利用 set 方法设置一些属性值。
- 如果 Bean 实现了 BeanNameAware 接口，调用 setBeanName()方法，传入 Bean 的名字。
- 如果 Bean 实现了 BeanClassLoaderAware 接口，调用 setBeanClassLoader()方法，传入 ClassLoader 对象的实例。
- 如果 Bean 实现了 BeanFactoryAware 接口，调用 setBeanClassLoader()方法，传入 ClassLoader 对象的实例。
- 与上面的类似，如果实现了其他\*Aware 接口，就调用相应的方法。
- 如果有和加载这个 Bean 的 Spring 容器相关的 BeanPostProcessor 对象，执行 postProcessBeforeInitialization()方法
- 如果 Bean 实现了 InitializingBean 接口，执行 afterPropertiesSet()方法。
- 如果 Bean 在配置文件中的定义包含`init-method`属性，执行指定的方法。
- 如果有和加载这个 Bean 的 Spring 容器相关的 BeanPostProcessor 对象，执行 postProcessAfterInitialization()方法
- 当要销毁 Bean 的时候，如果 Bean 实现了 DisposableBean 接口，执行 destroy()方法。
- 当要销毁 Bean 的时候，如果 Bean 在配置文件中的定义包含`destroy-method`属性，执行指定的方法。
