# EasyOps WebLogic 监控插件包

EasyOps WebLogic 监控插件包是适用于 EasyOps 新版监控平台，专门提供监控 WebLogic 服务的官方插件包。它提供了对 WebLogic 常见监控指标进行采集的采集插件以及可视化的仪表盘展示。

## 目录

- [背景](#背景)
- [适用环境](#适用环境)
- [工作原理](#工作原理)
- [准备工作](#准备工作)
- [使用方法](#使用方法)
- [项目内容](#项目内容)
- [维护者](#维护者)
- [许可证](#许可证)

## 背景

由于目前在 EasyOps 新版监控平台上搭建 WebLogic 监控场景需要经过以下步骤：

1. 自行搜索 WebLogic Exporter 并调试配置。
2. 在插件中心创建采集插件，使用步骤1输出的指标数据录入监控指标。
3. 使用创建的采集插件为具体的资源实例创建采集任务。
4. 理解监控指标含义后配置仪表盘展示。

所以为了实现 WebLogic 监控场景的快速搭建，该项目对 WebLogic 一些通用的监控指标及其采集脚本进行了封装，同时提供一个基本的仪表盘展示。

用户能够借助 EasyOps 平台提供的自动化工具来一键导入该插件包，真正做到 WebLogic 监控场景的开箱即用。

## 适用环境

WebLogic >= 12.1

## 工作原理

1. WebLogic 监控插件包使用了 WebLogic Exporter 进行指标采集，该 Exporter 的 GitHub 链接为 https://github.com/oracle/weblogic-monitoring-exporter 。

## 准备工作

1. 确认采集的 WebLogic 的访问地址和登录用户及密码。

## 使用方法

### 导入监控插件包

1. 下载该项目的压缩包 ( https://github.com/easy-monitor/weblogic-collector-plugin/archive/master.zip )。

2. 建议解压到 EasyOps 平台服务器上的 `/data/easyops/monitor_plugin_packages` 目录下。

3. 使用 EasyOps 平台提供的自动化工具一键导入该插件包，具体命令如下，请替换其中的 `8888` 为当前 EasyOps 平台具体的 `org`。

```sh
$ cd /usr/local/easyops/collector_plugin_service/tools
$ sh plugin_op.sh install 8888 /data/easyops/monitor_plugin_packages/weblogic-collector-plugin
```

4. 导入成功后访问 EasyOps 平台的「采集插件」列表页面 ( http://your-easyops-server/next/collector-plugin )，就能看到导入的 "weblogic_collector_plugin" 采集插件。

### 启动 WebLogic Exporter

1. 登录到 WebLogic 平台，将 `src/wls-exporter.war` 该应用部署到要监控的域下。

2. 修改 `conf/config.yml` 文件的 `query_sync` - `url` 为 WebLogic 具体的 URL。访问部署的 Exporter 应用，默认页面的 URI 为 `http://{weblogic_host}:{weblogic_port}/wls-exporter`，使用修改的 `config.yml` 进行提交后，访问 `http://{weblogic_host}:{weblogic_port}/wls-exporter/metrics` 就可获取到指标数据。

## 项目内容

### 目录结构

```
weblogic-collector-plugin
├── dashboard.json
├── origin_metric.json
└── script
    ├── conf
    │   └── config.yml
    ├── package.conf.yaml
    ├── plugin.yaml
    └── src
        └── wls-exporter.war
```

该项目的目录结构遵循标准的 EasyOps 监控插件包规范，具体内容如下：

- dashboard.json: 仪表盘的定义文件
- origin_metric.json: 采集插件关联的监控指标定义文件
- script: 采集插件关联的程序包目录，执行采集任务时会部署到指定的目标机器上
- script/conf: 配置文件目录
- script/package.conf.yaml: 采集插件关联的程序包的定义文件
- script/plugin.yaml: 采集插件包的定义文件
- script/src: 采集插件包的 Exporter 目录

### plugin.yaml

```yaml
# 支持 easyops/prometheus/zabbix-agent 三种采集类型
# 1. easyops: 表示使用 EasyOps Agent 进行指标采集
# 2. prometheus: 表示对接 Prometheus Exporter 进行指标采集
# 3. zabbix-agent: 表示对接 Zabbix Agent 进行指标采集
agentType: prometheus

# 采集插件的名称，也是采集插件关联的程序包名称
name: weblogic_collector_plugin
# 采集插件关联的程序包版本名称
version: 1.0.0

# 采集插件类别 
category: WEB框架
```

## 维护者

@easyopscyrilchen

## 许可证

[MIT](#许可证) © EasyOps
