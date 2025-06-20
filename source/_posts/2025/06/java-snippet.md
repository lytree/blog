---
title: java_snippet
date: 2025-06-20 14:48:20
updated:
tags: ['Java','Code Snippet']
categories: Java
---

```java
public abstract class BaseHandlerFactory<T> {

    protected final Map<String, T> handlers = new HashMap<>();

    public void register(String key, T handler) {
        handlers.put(key, handler);
    }

    public T get(String key) {
        return handlers.get(key);
    }

    public T exist(String key) {
        if (handlers.containsKey(key)) {
            return handlers.get(key);
        } else {
            return getDefaultHandler(key);
        }
    }

    public void del(String key) {
        this.handlers.remove(key);
    }

    protected abstract T getDefaultHandler(String key);

    public Set<String> keys() {
        return this.handlers.keySet();
    }

    protected abstract T getDefaultHandler();

}
```

```java
@Component
public class ApplicationContextProvider implements ApplicationContextAware, DisposableBean {
    private static ApplicationContext context;

    @Override
    public void setApplicationContext(ApplicationContext applicationContext) {
        ApplicationContextProvider.context = applicationContext;
    }

    public static ApplicationContext getApplicationContext() {
        return context;
    }


    /**
     * 从静态变量applicationContext中取得Bean, 自动转型为所赋值对象的类型.
     */
    @SuppressWarnings("unchecked")
    public static <T> T getBean(String name) {
        assertContextInjected();
        return (T) context.getBean(name);
    }

    /**
     * 从静态变量applicationContext中取得Bean, 自动转型为所赋值对象的类型.
     */
    public static <T> T getBean(Class<T> requiredType) {
        assertContextInjected();
        return context.getBean(requiredType);
    }

    /**
     * 从静态变量applicationContext中取得Bean, 自动转型为所赋值对象的类型.
     */
    public static <T> T getBean(String name, Class<T> requiredType) {
        assertContextInjected();
        return context.getBean(name, requiredType);
    }

    /**
     * 清除SpringContextHolder中的ApplicationContext为Null.
     */
    public static void clearHolder() {
        context = null;
    }

    /**
     * 实现DisposableBean接口, 在Context关闭时清理静态变量.
     */
    @Override
    public void destroy() {
        clearHolder();
    }

    /**
     * 检查ApplicationContext不为空.
     */
    private static void assertContextInjected() {
        if (context == null) {
            throw new IllegalStateException("applicaitonContext属性未注入, 请在applicationContext.xml中定义SpringContextHolder或在SpringBoot启动类中注册SpringContextHolder.");
        }
    }
}
```
