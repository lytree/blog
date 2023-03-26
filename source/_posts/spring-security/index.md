---
title: 'Spring Security'
description:
date: 2021-11-03T21:41:57+08:00
image:
math:
license:
categories:
  - spring security
tags:
  - spring
hidden: false
comments: false
draft: false
---

# Spring Security

<!--more-->

## Spring Seurity 工作流程

Spring Security 对 form 表单认证和 Basic 认证内置的两个 Filter
`formLogin` 对应着你 form 表单认证方式，即 `UsernamePasswordAuthenticationFilter`。
`httpBasic` 对应着 Basic 认证方式，即 `BasicAuthenticationFilter`。
`JWT`需自定义过滤器
[![guWC0x.jpg](https://z3.ax1x.com/2021/05/04/guWC0x.jpg)](https://imgtu.com/i/guWC0x)

## Spring Security 重要概念

### SecurityContext：上下文对象，Authentication 对象会放在里面。

```java
public interface SecurityContext extends Serializable {
 // 获取Authentication对象
 Authentication getAuthentication();

 // 放入Authentication对象
 void setAuthentication(Authentication authentication);
}
```

主要用于 get 或 set `Authentication`

### SecurityContextHolder：用于拿到上下文对象的静态工具类。

```java
public class SecurityContextHolder {

 public static void clearContext() {
  strategy.clearContext();
 }

 public static SecurityContext getContext() {
  return strategy.getContext();
 }

    public static void setContext(SecurityContext context) {
  strategy.setContext(context);
 }

}
```

用于 get or set or clear SecurityContext，默认会把数据都存储到当前线程中。

### Authentication：认证接口，定义了认证对象的数据形式。

```java
public interface Authentication extends Principal, Serializable {

 Collection<? extends GrantedAuthority> getAuthorities();
 Object getCredentials();
 Object getDetails();
 Object getPrincipal();
 boolean isAuthenticated();
 void setAuthenticated(boolean isAuthenticated) throws IllegalArgumentException;
}

```

- getAuthorities: 获取用户权限，一般情况下获取到的是用户的角色信息。
- getCredentials: 获取证明用户认证的信息，通常情况下获取到的是密码等信息。
- getDetails: 获取用户的额外信息，（这部分信息可以是我们的用户表中的信息）。
- getPrincipal: 获取用户身份信息，在未认证的情况下获取到的是用户名，在已认证的情况下获取到的是 UserDetails。
- isAuthenticated: 获取当前 Authentication 是否已认证。
- setAuthenticated: 设置当前 Authentication 是否已认证（true or false）。

Authentication 只是定义了一种在 SpringSecurity 进行认证过的数据的数据形式应该是怎么样的，要有权限，要有密码，要有身份信息，要有额外信息。

### AuthenticationManager：用于校验 Authentication，返回一个认证完成后的 Authentication 对象。

```java
public interface AuthenticationManager {
 // 认证方法
 Authentication authenticate(Authentication authentication)
   throws AuthenticationException;
}

```

AuthenticationManager 定义了一个认证方法，它将一个未认证的 Authentication 传入，返回一个已认证的 Authentication，默认使用的实现类为：ProviderManager。

### 认证流程

1. 👉 先是一个请求带着身份信息进来
2. 👉 经过 AuthenticationManager 的认证，
3. 👉 再通过 SecurityContextHolder 获取 SecurityContext，
4. 👉 最后将认证后的信息放入到 SecurityContext。
