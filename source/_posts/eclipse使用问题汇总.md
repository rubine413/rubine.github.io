---
title: eclipse使用问题汇总
date: 2013-05-03 10:42:21
description: 记录使用eclipse开发过程中遇到的各种问题
tags:
  - java
  - eclipse
---

```sequence
participant 客户端 as A
participant 服务端 as B
participant 通行证中心 as C
Note over A:用户输入通行证账号、密码
A->C: 发送账号、密码
Note over C:验证账号、密码
C-->>A:返回token
A->B:发送token
B->C:验证token
C-->>B:验证成功
B-->>A:登陆成功
Note left of A:左边注释
B->B:自交互
Note right of C:右边注释
```

```flow
st=>start: 开始
io=>inputoutput: 验证
op=>operation: 选项
cond=>condition: 是 或 否？
sub=>subroutine: 子程序
e=>end: 结束
st->io->op->cond
cond(yes)->e
cond(no)->sub->io
```
