---
layout: post
title: centos7でcronを時間通り動かす
categories: os
tags: cron anacron
---

## anacron

centos7デフォルトだとanacronが動くため設定した時間通りにcronが実行されない

## 通常のcronを入れなおす

noanacronをインストールし、anacronをインストールする（この順番で入れる必要がある）

```
sudo yum -y install cronie-noanacron
sudo yum -y remove cronie-anacron
```

インストール後は再起動が必要

```
sudo systemctl restart crond
```

下記設定ファイルに実行したジョブを記載

```
sudo vi /etc/cron.d/dailyjobs

# Run the daily, weekly, and monthly jobs if cronie-anacron is not installed
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root

# run-parts
2 4 * * * root [ ! -f /etc/cron.hourly/0anacron ] && run-parts /etc/cron.daily
22 4 * * 0 root [ ! -f /etc/cron.hourly/0anacron ] && run-parts /etc/cron.weekly
42 4 1 * * root [ ! -f /etc/cron.hourly/0anacron ] && run-parts /etc/cron.monthly
```








