---
title: ssh
date: 2016-11-15 16:41:58
tags: ssh, backend
---

## 什么是ssh

ssh是用来登录远程服务器并进行操作的程序。

```bash
ssh [-1246AaCfgkMNnqsTtVvXxY] [-b bind_address] [-c cipher_spec]
         [-D port] [-e escape_char] [-F configfile] [-i identity_file] [-L
         [bind_address:]port:host:hostport] [-l login_name] [-m mac_spec]
         [-O ctl_cmd] [-o option] [-p port] [-R
         [bind_address:]port:host:hostport] [-S ctl_path] [user@]hostname
         [command]
```

## 公钥(public key)验证

ssh客户端发送它的私钥(private key)， ~/.ssh/id_dsa 或者 ~/.ssh/id_rsa到远程服务器。 远程服务器检测~/.ssh/authorized_keys是否有对应的公钥(public key)。

<!-- more -->

## 客户端配置

修改 ~/.ssh/config文件

```
Host remoteServer               # 方便记忆的主机登录伪名
HostName www.google.com         # 服务器ip或域名
User root                       # 登录的用户名
IdentityFile ~/.ssh/id_rsa      # 私钥路径
```

id_rsa(私钥private key)和id_rsa.pub(公钥public key)一一对应

## 直接修改远程服务器文件

### mount

1. 安装FUSE和SSHFS: https://osxfuse.github.io/
2.
```
sshfs root@server.host.xxx.com:/ /local/directory/to/mount
```

### unmount
```
umount /local/directory/to/mount
```

## 相关资料：
[文档](http://linuxcommand.org/man_pages/ssh1.html)

