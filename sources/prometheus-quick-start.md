# 初识Prometheus

这一个小节，我们将带来读者在本地基于搭建一个单实例的Prometheus Server,并且基于它我们快快速认识和熟悉一下Prometheus。

## 环境准备

我们使用[Vagrant](https://www.vagrantup.com)创建了一个ubuntu/xenial64本地的虚拟机。我们将在该环境下安装部署Prometheus

这里我们使用Vagrantfile来定义我们的基础环境，我们使用ubuntu/xenial64作为基础镜像，并且为虚拟机分配了一个私有的IP地址192.168.33.10，通过该IP地址我们可以直接在本地访问运行在该虚拟机中的服务。

Vagrantfile内容如下：

``` Vagrantfile
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.network "private_network", ip: "192.168.33.10"
end

```

启动虚拟机

```
$ vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Clearing any previously set forwarded ports...
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
    default: Adapter 2: hostonly
==> default: Forwarding ports...
    default: 22 (guest) => 2222 (host) (adapter 1)
==> default: Running 'pre-boot' VM customizations...
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: ubuntu
    default: SSH auth method: password
==> default: Machine booted and ready!
[default] GuestAdditions 5.1.30 running --- OK.
==> default: Checking for guest additions in VM...
==> default: Configuring and enabling network interfaces...
==> default: Mounting shared folders...
    default: /Workspace/books/prometheus-in-action/examples/ch1
==> default: Machine already provisioned. Run `vagrant provision` or use the `--provision`
==> default: flag to force provisioning. Provisioners marked to run always will still run.
```

启动完成后，我们可以通过命令vagrant ssh登录到该虚拟机

```
$ vagrant ssh
Welcome to Ubuntu 16.04.2 LTS (GNU/Linux 4.4.0-112-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

96 packages can be updated.
1 update is a security update.


Last login: Wed Jan 31 01:41:40 2018 from 10.0.2.2
ubuntu@ubuntu-xenial:~$
```

## 如何获取二进制包

Prometheus的源代码托管在github的仓库[https://github.com/prometheus/prometheus](https://github.com/prometheus/prometheus)在下，在[releases](https://github.com/prometheus/prometheus/releases)链接下我们可以找到Prometheus所有已发布版本的软件包。

## 安装Prometheus Server

### 创建本地用户

```
sudo useradd --no-create-home prometheus

sudo mkdir /etc/prometheus
sudo mkdir /var/lib/prometheus

sudo chown prometheus:prometheus /etc/prometheus
sudo chown prometheus:prometheus /var/lib/prometheus
```

### 获取并安装软件包

```
cd ~
curl -LO https://github.com/prometheus/prometheus/releases/download/v2.0.0/prometheus-2.0.0.linux-amd64.tar.gz
tar xvf prometheus-2.0.0.linux-amd64.tar.gz
```

```
sudo cp prometheus-2.0.0.linux-amd64/prometheus /usr/local/bin/
sudo chown prometheus:prometheus /usr/local/bin/prometheus
```

### 创建Prometheus配置文件

创建配置文件：/etc/prometheus/prometheus.yml

```
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    scrape_interval: 5s
    static_configs:
      - targets: ['localhost:9090']
```

```
$ sudo chown prometheus:prometheus /etc/prometheus/prometheus.yml
```

这里我们让Prometheus Server每15秒，轮训一次[http://localhost:9090/metrics](http://localhost:9090/metrics)Prometheus Server自己暴露的监控指标。

### 运行Prometheus

```
$ sudo -u prometheus /usr/local/bin/prometheus \
  --config.file /etc/prometheus/prometheus.yml \
  --storage.tsdb.path /var/lib/prometheus/
```

```
level=info ts=2018-02-05T05:58:12.438943323Z caller=main.go:215 msg="Starting Prometheus" version="(version=2.0.0, branch=HEAD, revision=0a74f98628a0463dddc90528220c94de5032d1a0)"
level=info ts=2018-02-05T05:58:12.439697472Z caller=main.go:216 build_context="(go=go1.9.2, user=root@615b82cb36b6, date=20171108-07:11:59)"
level=info ts=2018-02-05T05:58:12.4403636Z caller=main.go:217 host_details="(Linux 4.4.0-112-generic #135-Ubuntu SMP Fri Jan 19 11:48:36 UTC 2018 x86_64 ubuntu-xenial (none))"
level=info ts=2018-02-05T05:58:12.445696612Z caller=web.go:380 component=web msg="Start listening for connections" address=0.0.0.0:9090
level=info ts=2018-02-05T05:58:12.445720858Z caller=main.go:314 msg="Starting TSDB"
level=info ts=2018-02-05T05:58:12.445785952Z caller=targetmanager.go:71 component="target manager" msg="Starting target manager..."
level=info ts=2018-02-05T05:58:12.761069867Z caller=main.go:326 msg="TSDB started"
level=info ts=2018-02-05T05:58:12.762288533Z caller=main.go:394 msg="Loading configuration file" filename=/etc/prometheus/prometheus.yml
level=info ts=2018-02-05T05:58:12.764189762Z caller=main.go:371 msg="Server is ready to receive requests.
```

### 创建Prometheus Server的Service Unit文件

```
sudo vim /etc/systemd/system/prometheus.service
```

```
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
    --config.file /etc/prometheus/prometheus.yml \
    --storage.tsdb.path /var/lib/prometheus/ \
    --web.console.templates=/etc/prometheus/consoles \
    --web.console.libraries=/etc/prometheus/console_libraries

[Install]
WantedBy=multi-user.target
```

```
sudo systemctl daemon-reload
sudo systemctl status prometheus
sudo systemctl enable prometheus
sudo systemctl restart prometheus
```

## 使用Expression Browser

目前为止我们即完成了Prometheus Server的部署，在Prometheus启动完成后，访问[http://192.168.33.10:9090/](http://192.168.33.10:9090/)即可访问Prometheus内置的UI程序Expression Browser。

![](http://p2n2em8ut.bkt.clouddn.com/prometheus-ui-graph.png)

访问[http://192.168.33.10:9090/metrics](http://192.168.33.10:9090/metrics),我们可以查看当前Prometheus Server自身的监控指标数据。



![](http://p2n2em8ut.bkt.clouddn.com/prometheus-metrics.png)

回到Expression Browser，[http://192.168.33.10:9090/graph](http://192.168.33.10:9090/graph)，并切换到Console标签。我们可以通过,输入表达式http_requests_total来查看Prometheus Server的Http请求量的情况。在UI中输入表达式

```
http_requests_total
```

![](http://p2n2em8ut.bkt.clouddn.com/prometheus_ui_http_request_total_console.png)

我们还可以通过返回数据样本中标签对数据进行过滤，例如我们只关心handler=query的请求次数。那我们可以通过表达式：

```
http_requests_total{handler='query'}
```

如果只需要计算当前表达式返回的时间序列的条数则可以使用count函数进行计算。

```
count(http_requests_total)
```

### 使用统计图表

Prometheus的Expression Browser的UI中，我们还可以直接通过表达式计算实时产生统计图表.回到Expression Browser，[http://192.168.33.10:9090/graph](http://192.168.33.10:9090/graph)，并切换到Graph标签。

输入以下表达式，我们可以统计当前Prometheus Server每秒接收的请求次数速率。

```
rate(http_requests_total[1m])
```

![](http://p2n2em8ut.bkt.clouddn.com/prometheus_ui_http_request_graph.png)