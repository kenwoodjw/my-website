---
title: "Loki"
date: 2020-07-25T16:20:35+08:00
draft: false
author: "kenwood"
isCJKLanguage: true
tags: ["loki", "prometheus"]
categories: ["grafana"]
---

### loki 日志采集安装

#### 下载loki和promtail

```shell
wget https://github.com/grafana/loki/releases/download/v1.5.0/loki-linux-amd64.zip
uznip loki-linux-amd64.zip 
mv loki-linux-amd64 /usr/local/bin/loki
wget https://github.com/grafana/loki/releases/download/v1.5.0/promtail-linux-amd64.zip
unzip promtail-linux-amd64.zip
unzip promtail-linux-amd64 /usr/local/bin/promtail

```

#### 配置文件

https://github.com/grafana/loki/blob/v1.5.0/cmd/loki/loki-local-config.yaml

https://github.com/grafana/loki/blob/v1.5.0/cmd/promtail/promtail-local-config.yaml

```
mv loki-local-config.yaml /usr/local/bin/config-loki.yml
mv promtail-local-config.yaml /usr/local/bin/config-promtail.yml
```

#### 配置systemd 

```
vi /etc/systemd/system/loki.service
[Unit]
Description=Loki service
After=network.target

[Service]
Type=simple
ExecStart=/usr/local/bin/loki -config.file /usr/local/bin/config-loki.yml

[Install]
WantedBy=multi-user.target
```

```
vi /etc/systemd/system/promtail.service
[Unit]
Description=Promtail service
After=network.target

[Service]
Type=simple
ExecStart=/usr/local/bin/promtail -config.file /usr/local/bin/config-promtail.yml

[Install]
WantedBy=multi-user.target
```

```
systemctl start loki
systemctl enable loki
systemctl start promtail
systemctl start promtail

```

#### 验证

[http://[Your](http://[your/)-Server-Domain-or-IP]:3100/metrics

[http://[Your](http://[your/)-Server-Domain-or-IP]:9080

#### grafana 数据源添加

添加数据源

http://127.0.0.1:3100

点击查看exporter