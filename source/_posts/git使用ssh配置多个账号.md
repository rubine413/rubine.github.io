---
title: git使用ssh配置多个账号
date: 2017-05-12 10:22:02
tags: git
categories: git
toc: true
---
#### 生成本地密钥

假如我们有2个git账号，分别来自[github](https://www.github.com/)和[coding](https://coding.net/)，进入目录C:\Users\Administrator\\.ssh

首先生成github密钥，注意区分密钥文件名
```bash
ssh-keygen -t rsa -C "email@gmail.com" -f id_rsa_github
```

生成coding密钥
```bash
ssh-keygen -t rsa -C "email@qq.com" -f id_rsa_coding
```
 
生成结果如下所示，以pub结尾的就是公钥文件
```bash
-rw-------+ 1 Administrator None 1679 Dec 19 12:17 id_rsa_coding
-rw-r--r--+ 1 Administrator None  397 Dec 19 12:17 id_rsa_coding.pub
-rw-------+ 1 Administrator None 1679 Dec 17 10:29 id_rsa_github
-rw-r--r--+ 1 Administrator None  399 Dec 17 10:29 id_rsa_github.pub
```

---  
#### 添加私钥
```bash
ssh-add id_rsa_github
ssh-add id_rsa_coding
```

如果返回`could not open a connection to your authentication agent`, 则先执行以下命令
```bash
ssh-agent bash
```
<!--more-->
---  
#### 创建配置文件
在.ssh目录下新建配置文件config
```bash
touch config
```

并添加以下配置内容:
```bash
# *配置github.com*
Host github.com              
    HostName github.com
    IdentityFile C:\\Users\\Administrator\\.ssh\\id_rsa_github
    PreferredAuthentications publickey
    User git

# 配置git.coding.net 
Host coding.net
    HostName coding.net
    IdentityFile C:\\Users\\Administrator\\.ssh\\id_rsa_coding
    PreferredAuthentications publickey
    User git
```

---
#### 上传公钥
以github为例, 进入Settings -> SSH and GPG keys, 点击"New SSH key", 并将id_rsa_github.pub文件里的内容复制到Key中
![图片](https://image.xiaoxiaojiang.com/site_20181219133650.png)  

---
#### 测试结果
```bash
ssh -T git@github.com
```

如果返回以下结果表示配置成功
```
Hi xxx! You've successfully authenticated, but GitHub does not provide shell access.
```