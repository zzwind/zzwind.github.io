---
title: Nginx Brotli 压缩 TTFB时间高排查
date: 2020-04-17 11:17:45
tags: ['Nginx','运维']
---

## 起因
因为内部系统的网络传输负载过高，决定使用 `Brotli` 压缩内容。

## 处理过程
### PHP端
安装brotli php扩展
配置：
```bash
brotli.output_compression=1
brotli.output_compression_level=9
```

### Nginx 
安装 `ngx_brotli` 模块 [ngx_brotli](https://github.com/google/ngx_brotli)
配置：
```bash
brotli on; 
brotli_comp_level 9; 
brotli_buffers 16 8k; 
brotli_min_length 20; 
brotli_types *; 
brotli_window 16m;
```

重启nginx && php-fpm

结果TTFB的时间一直为300ms+，原因是因为 `nginx` 配置里面的 `brotli_comp_level` 设为了 11 ，压缩等级太高导致nginx的CPU占用过高。而我自己一直在调整PHP端的 `brotli.output_compression_level` 压缩参数。导致nginx占用一直居高不下。 

顺便测试了一下 `Brotli` 的压缩等级1-11，11是完全没有必要的。9是最合适的压缩等级。