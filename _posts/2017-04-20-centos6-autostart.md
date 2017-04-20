---
layout: post
title: centos6の自動起動関連の設定について
categories: os
tags: centos
---

現在の自動起動設定の確認

```
/sbin/chkconfig --list
```

httpdをランレベル3と5で自動起動するように設定

```
/sbin/chkconfig --level 35 httpd on
```

現在のランレベル確認

```
/sbin/runlevel
```

ランレベルの設定を確認

```
cat /etc/inittab
```

/etc/inittab記載のランレベル説明

```
# Default runlevel. The runlevels used by RHS are:
#   0 - halt (Do NOT set initdefault to this)
#   1 - Single user mode
#   2 - Multiuser, without NFS (The same as 3, if you do not have networking)
#   3 - Full multiuser mode
#   4 - unused
#   5 - X11
#   6 - reboot (Do NOT set initdefault to this)
```

