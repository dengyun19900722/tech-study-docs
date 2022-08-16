# Alibaba iLogtail - 阿里轻量级遥测数据采集端 

​		iLogtail 为可观测场景而生，拥有的轻量级、高性能、自动化配置等诸多生产级别特性，在阿里巴巴以及外部数万家阿里云客户内部广泛应用。你可以将它部署于物理机，虚拟机，Kubernetes等多种环境中来采集遥测数据，例如logs、traces和metrics。

**iLogtail** 的核心优势主要有：

- 支持**多种Logs、Traces、Metrics**数据采集，尤其对容器、Kubernetes环境支持非常友好
- 数据采集资源消耗极低，相比同类遥测数据采集的Agent性能好5-20倍
- 高稳定性，在阿里巴巴以及数万阿里云客户生产中使用验证，部署量近千万，每天采集数十PB可观测数据
- 支持插件化扩展，可任意扩充数据采集、处理、聚合、发送模块
- 支持配置远程管理，支持以图形化、SDK、K8s Operator等方式进行配置管理，可轻松管理百万台机器的数据采集
- 支持自监控、流量控制、资源控制、主动告警、采集统计等多种高级特性

**iLogtail** 支持收集多种遥测数据并将其传输到多种不同的后端，例如 [SLS可观测平台](https://help.aliyun.com/product/28958.html) 。 支持采集的数据主要如下:

- Logs
  - 收集静态日志文件
  - 在容器化环境中运行时动态收集文件
  - 在容器化环境中运行时动态收集 Stdout
- Traces
  - OpenTelemetry 协议
  - Skywalking V2 协议
  - Skywalking V3 协议
  - ...
- Metrics
  - Node指标
  - Process指标
  - GPU 指标
  - Nginx 指标
  - 支持获取Prometheus指标
  - 支持收集Telegraf指标
  - ...