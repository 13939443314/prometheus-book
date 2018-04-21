# 目录

- [Introduction](README.md)
- [作者](AUTHOR.md)
- [版本变更历史](CHANGELOGS.md)
- [第1章 天降奇兵](./chapter0/README.md)
  - [Prometheus简介](./sources/why-monitor.md)
  - [初识Prometheus](./sources/prometheus-quick-start.md)
  - [任务和实例](./sources/prometheus-job-and-instance.md)
  - [Prometheus核心组件](./sources/prometheus-architecture-and-components.md)
  - [小结](./chapter0/SUMMARY.md)
- [第2章 探索PromQL](./chapter2/README.md)
  - [理解时间序列](./sources/what-is-prometheus-metrics-and-labels.md)
  - [Metrics类型](./sources/prometheus-metrics-types.md)
  - [初识PromQL](./sources/prometheus-query-language.md)
  - [PromQL操作符](./sources/prometheus-promql-operators.md)
  - [PromQL内置函数](./sources/prometheus-promql-functions.md)
  - [在HTTP API中使用PromQL](./sources/prometheus-promql-with-http-api.md)
  - [最佳实践：4个黄金指标和USE方法](./sources/prometheus-promql-best-praticase.md)
  - [小结](./chapter2/SUMMARY.md)
- [第3章 Prometheus告警处理](./chapter3/README.md)
  - [Prometheus告警简介](./sources/prometheus-alert-manager-overview.md)
  - [自定义Prometheus告警规则](./sources/prometheus-alert-rule.md)
  - [部署AlertManager](./sources/install-alert-manager.md)
  - [基于Label的动态告警处理](./sources/alert-manager-routes.md)
  - [内置告警接收器Receiver](./sources/alert-manager-with-smtp.md)
  - [使用Webhook扩展Alertmanager](./sources/alert-manager-extension-with-webhook.md)
  - [屏蔽告警通知](./sources/alert-manager-inhibit.md)
  - [使用Recoding Rules优化性能](./sources/prometheus-recoding-rules.md)
  - [小结](./chapter3/SUMMARY.md)
- [第4章 使用Exporter](./chapter5/README.md)
  - [Exporter是什么](./sources/what-is-prometheus-exporter.md)
  - [监控容器运行状态](./sources/use-prometheus-monitor-container.md)
  - [监控MySQL运行状态](./sources/use-promethues-monitor-mysql.md)
  <!-- - [监控Nginx运行状态](./sources/use-prometheus-monitor-nginx.md) -->
  <!-- [使用PrometheusRabbitMQ运行状态](./sources/use-prometheus-monitor-rabbitmq.md) -->
  - [扩展Prometheus](./sources/custom_metrics_with_java_sdk.md)
  - [在Spring Boot中集成Prometheus](./sources/custom_app_support_prometheus.md)
  <!-- - 使用Pushgateway -->
  <!-- - 垮网络监控 -->
  - [小结](./chapter5/SUMMARY.md)
- [第5章 可视化一切](./chapter4/README.md)
  - [Grafana简介](./sources/grafana-intro.md)
  - [安装Grafana]
        - 使用二进制包安装Grafana
        - 使用容器安装Grafana
  - 使用Prometheus数据源
  - 创建监控Dashboard
  - 自定义Panel
  - 标签
  - 表格
  - 注解
  - 使用模板
  - Repeat Panel
  - 共享Dashboard
  - 时间范围
  - SingleStat面板
  - [小结](./chapter5/SUMMARY.md)
- [第6章 集群与高可用](./chapter7/READMD.md)
  - [本地存储](./sources/prometheus-local-storage.md)
  - [远程存储](./sources/prometheus-remote-storage.md)
  - [联邦集群](./sources/scale-prometheus-with-federation.md)
  - [Prometheus高可用](./sources/prometheus-and-high-availability.md)
  - [Alertmanager高可用](./sources/alertmanager-high-availability.md)
  <!-- - 使用Prometheus Opertor管理Prometheus -->
  <!-- - 使用Promgen管理Prometheus -->
  <!-- - 从1.0迁移到2.0 -->
  - [总结](./chapter4/SUMMARY.md)
- [第7章 Prometheus服务发现](./chapter6/README.md)
  - [为什么需要服务发现](./sources/why-need-service-discovery.md)
  - [基于文件的服务发现](./sources/service-discovery-with-file.md)
  - [基于Consul的服务发现](./sources/service-discovery-with-consul.md)
  <!-- - [基于DNS的服务发现机制](./sources/service-discovery-with-dns.md) -->
  <!-- - [基于EC2的服务发现机制](./sources/service-discovery-with-ec2.md) -->
  - [Kubernetes下的服务发现](./sources/service-discovery-with-k8s.md)
  - [服务发现与Relabel](./sources/service-discovery-with-relabel.md)
  - [小结](./chapter6/SUMMARY.md)
- [第8章 使用BlackBox监控网络](./chapter9/README.md)
  - [黑盒监控和Blackbox Exporter](./sources/install_blackbox_exporter.md)
  - [HTTP探针](./sources/blackbox-http-probe.md)
  - [TCP探针](./sources/blackbox-tcp-probe.md)
  - [DNS探针](./sources/blackbox-dns-probe.md)
  - [ICMP探针](./sources/blackbox-icmp-probe.md)
  - [IPv4和IPv6](./sources/blackbox-ipvx-probe.md)
  - 小结
- [第9章 使用Prometheus监控Kubernetes集群](./chapter8/READMD.md)
  - Kubernetes简介
  - 搭建Kubernetes本地测试环境
  - Prometheus Vs Heapster
  - 在Kubernetes下部署Prometheus
  - 采集Kubelet状态
  <!-- - [采集集群状态](./sources/expose-cluster-level-metrics-with-kube-state-metrics.md) -->
  - 采集应用资源用量
  - 采集集群节点状态指标
  - 使用Grafana创建可视化仪表盘
  - 基于Prometheus实现应用的弹性伸缩
  - 使用Prometheus Opertor管理Prometheus
  - 总结
- [第10章 使用Prometheus监控Rancher集群](./chapter10/README.md)
- [参考资料](./REFERENCES.md)