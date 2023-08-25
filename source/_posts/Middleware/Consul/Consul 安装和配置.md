---
title: Consul 安装和配置
date: 2022-11-12T14:30:37Z
lastmod: 2022-11-12T14:31:54Z
---

# Consul 安装和配置

## 服务端启动命令

```shell
consul agent -server -bootstrap-expect 1 -data-dir /home/ubuntu/consul/data -ui -config-dir /home/ubuntu/consul/consul.d -bind=172.25.74.176 -client=0.0.0.0
```

　　‍
