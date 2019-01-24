## Elastic Stack 介绍

### 同类型产品对比
> 对比 Hadoop、Spark、Storm、Hive，主要的特点是快、快、快！

- 使用门槛低，开发周期短，上线快
- 性能好，查询快，实时展示结果
- 扩容方便，快速支撑增长迅猛的数据

### 主要包含的内容
> ELK 指 Elasticsearch、logstash、Kibana

- Kibana
- Elasticsearch
- Beats
- Logstash

#### Elasticsearch 
- Elastic Stack 的核心引擎

#### Beats Logstash
- 数据收集与处理
- 类似 ETL 工具 
	- Extract Transform Load
- 数据源多样
	- 数据文件，如 日志、Excel 等
	- 数据库，如 MySQL， Oracle 等
	- http 服务
	- 网络数据
- 支持自定义扩展，无限可能

#### Kibana
- 数据探索与可视化分析

### 应用场景
> 完备的数据分析工具集合

- 搜索引擎
- 日志分析
- 指标分析