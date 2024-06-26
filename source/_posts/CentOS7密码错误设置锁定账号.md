---
title: CentOS7密码错误设置锁定账号
date: 2020-11-14 15:30:08
tags: [服务器]
---

**1、限制用户远程登录**

在#%PAM-1.0的下面，即第二行，添加内容，一定要写在前面，如果写在后面，虽然用户被锁定，但是只要用户输入正确的密码，还是可以登录的！

```
vim /etc/pam.d/sshd
```

```
#%PAM-1.0  

auth required pam_tally2.so deny=3 unlock_time=300 even_deny_root root_unlock_time=10
```

参数解释

even_deny_root 也限制root用户；

deny 设置普通用户和root用户连续错误登陆的最大次数，超过最大次数，则锁定该用户；

unlock_time 设定普通用户锁定后，多少时间后解锁，单位是秒；

root_unlock_time 设定root用户锁定后，多少时间后解锁，单位是秒；

此处使用的是 pam_tally2 模块，如果不支持 pam_tally2 可以使用 pam_tally 模块。另外，不同的pam版本，设置可能有所不同，具体使用方法，可以参照相关模块的使用规则。

**2、限制用户从tty登录**

在#%PAM-1.0的下面，即第二行，添加内容，一定要写在前面，如果写在后面，虽然用户被锁定，但是只要用户输入正确的密码，还是可以登录的！

```
vim /etc/pam.d/login
```

```
#%PAM-1.0  

auth required pam_tally2.so deny=3 lock_time=300 even_deny_root root_unlock_time=10
```

同样是增加在第2行！

**3、查看用户登录失败次数**

```
cd /etc/pam.d/
pam_tally2 --user root
```

![](1.png)

**4、解锁指定用户**

```
pam_tally2 -r -u root
```

