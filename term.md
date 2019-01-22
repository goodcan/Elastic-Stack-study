## Elastic Stack 常用术语

### 文档 Document
> 用户存储在 es 中的数据文档

- Json Object，有字段（Field）组成，常见数据类型如下：
	- 字符串：text，keyword
	- 数值型：long integer，short byte，double，float，half_float，scaled_float
	- 布尔：boolean
	- 日期：date
	- 二进制：binary
	- 范围类型：integer_range，float_range，long_range，double_range，date_range
- 每个文档有唯一的 id 标识
	- 自行制定
	- es 自动生成
- 元数据（MetaData），用于标注文档的相关细腻
	- \_index：文档所在的索引名
	- \_type：文档所在的类型名
	- \_id: 文档唯一 id
	- \_uid 组合 id，有 \_type 和 \_id 组成（6.x \_type 不再起作用，同 \_id 一样）
	- \_source：文档的原始 Json 数据，可以从这里获取每个字段的内容
	- \_all：整合所有字段内容到该字段，默认禁用

### 索引 Index
> 由具有相同字段的文档列表组成

- 索引中存储具有相同结构的文档（document）
	- 每个索引都有自己的 mpping 定义，用于定义字段名和类型
- 一个集群可以有多个索引，比如：
	- nginx 日志存储的时候可以按照日期每天生成一个索引来存储
		- nginx-log-2017-01-01
		- nginx-log-2017-01-02
		- nginx-log-2017-01-03

### 节点 Node
- 一个 Elasticsearch 的运行实例，是集群的构成单元

### 集群 Cluster
- 由一个或多个节点组成，对外提供服务