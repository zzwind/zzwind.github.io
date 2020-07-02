---
title: centos7安装docker-ce
date: 2020-07-02 09:09:04
tags:
---

```bash
ansible testxxl -m shell -a "yum install -y yum-utils"
ansible testxxl -m shell -a "yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo"
ansible testxxl -m shell -a "yum install -y docker-ce"
ansible testxxl -m shell -a "systemctl start docker"
ansible testxxl -m shell -a "systemctl enable docker"
```