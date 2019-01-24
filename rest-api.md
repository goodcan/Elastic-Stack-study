## Elastic Stack Rest API

### Elasticsearch 集群对外提供 RESTFUL API
- REST - REpresentational State Transfer
- URI 指定资源，如 Index、Document 等
- Http Method 指明资源操作类型，如 GET、POST、PUT、DELETE 等

### 常用两种交互方式
- Curl 命令行
- Kibana DevTools

### 索引 API
> es 有专门的 Index API，用于创建、更新、删除索引配置等

#### 创建索引 api
> 格式：PUT /index-name

```
# request
PUT /demo

# response
{
  "acknowledged" : true,
  "shards_acknowledged" : true,
  "index" : "demo"
}
```

### 删除索引 api
> 格式：DELETE /index-name

```
# request
DELETE /demo

# respoonse
{
  "acknowledged" : true
}
```

### 文档 API
#### 创建文档
##### 指定 id 创建文档 api
> 格式：PUT /index-name/type-name/id-name http-body  
> 创建文档时，如果索引不存在，es 会自动创建对应的 index 和 type  
> type 写 doc 即可，之后的版本会废弃 type

```
# request
PUT /demo/doc/1
{
    "username": "dome_1",
    "age": 1
}

# response
{
  "_index" : "demo",
  "_type" : "doc",
  "_id" : "1",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 0,
  "_primary_term" : 1
}
```

##### 不指定 id 创建文档 api
> 格式 POST /index-name/type-name http-body  
> es 自动生成 id

```
# request
POST /demo/doc
{
    "username": "demo_2",
    "age": 2
}

# response
{
  "_index" : "demo",
  "_type" : "doc",
  "_id" : "ocpIe2gBMnTX0h4G92JW",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 0,
  "_primary_term" : 1
}
```
##### 批量创建文档
> 格式：POST /\_bulk http-body  
> es 允许一次创建多个文档，从而减少网络传输开销提升写入速率  
> <font color="#dd0000">\_bulk 要求每个 json 串不能换行</font>

```
# request
# action_type: index、update、create、delete
# index 和 create 都是创建，create 在创建时如果已经存在则会报错，index 会覆盖原文档
# _bulk 中每个 json 数据不能为空
POST /_bulk
{"index":{"_index":"demo","_type":"doc","_id":"3"}}
{"usernae":"demo_3","age":3}
{"delete":{"_index":"demo","_type":"doc","_id":"1"}}
{"update":{"_index":"demo","_type":"doc","_id":"2"}}
{"doc":{"age":20}}

# response
{
  "took" : 170,
  "errors" : false,
  "items" : [
    {
      "index" : {
        "_index" : "demo",
        "_type" : "doc",
        "_id" : "3",
        "_version" : 1,
        "result" : "created",
        "_shards" : {
          "total" : 2,
          "successful" : 1,
          "failed" : 0
        },
        "_seq_no" : 0,
        "_primary_term" : 4,
        "status" : 201
      }
    },
    {
      "delete" : {
        "_index" : "demo",
        "_type" : "doc",
        "_id" : "1",
        "_version" : 3,
        "result" : "deleted",
        "_shards" : {
          "total" : 2,
          "successful" : 1,
          "failed" : 0
        },
        "_seq_no" : 2,
        "_primary_term" : 4,
        "status" : 200
      }
    },
    {
      "update" : {
        "_index" : "demo",
        "_type" : "doc",
        "_id" : "2",
        "_version" : 2,
        "result" : "updated",
        "_shards" : {
          "total" : 2,
          "successful" : 1,
          "failed" : 0
        },
        "_seq_no" : 1,
        "_primary_term" : 4,
        "status" : 200
      }
    }
  ]
}

```

#### 查询文档
##### 指定文档 id 查询
> 格式：GET /index-name/type-name/id-name  
> \_source 存储原始数据

```
# reqeust
GET /deom/doc/1

# response
{
  "_index" : "demo",
  "_type" : "doc",
  "_id" : "1",
  "_version" : 2,
  "found" : true,
  "_source" : {
    "username" : "dome_1",
    "age" : 1
  }
}
```

##### 搜索所有文档，使用 \_search
> 格式：GET /index-name/type-name/\_search http-body
> 查询语句，Json 格式，放在 http body 中发送到 es  
> 不加 http body 返回默认前十个文档  

```
# request
GET /demo/doc/_search

# response
# took 查询耗时，单位 ms
# hits 查询命中内容 
# total 符合条件的总文档数
# hits.hits 返回的文档详情数据数组，默认前 10 个文档
# _id 文档的 id
# _index 索引名
# _score 文档的得分
# _source 文档详情
{
  "took" : 14,  
  "timed_out" : false,
  "_shards" : {
    "total" : 5,
    "successful" : 5,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : 2,    
    "max_score" : 1.0,
    "hits" : [  
      {
        "_index" : "demo",  
        "_type" : "doc",
        "_id" : "ocpIe2gBMnTX0h4G92JW", 
        "_score" : 1.0, 
        "_source" : { 
          "username" : "dome_2",
          "age" : 1
        }
      },
      {
        "_index" : "demo",
        "_type" : "doc",
        "_id" : "1",
        "_score" : 1.0,
        "_source" : {
          "username" : "dome_1",
          "age" : 1
        }
      }
    ]
  }
}

# request
GET /demo/doc/_search
{
    "query": {
        "term": {
            "_id": "1"
        }
    }
}

# response
{
  "took" : 13,
  "timed_out" : false,
  "_shards" : {
    "total" : 5,
    "successful" : 5,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : 1,
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "demo",
        "_type" : "doc",
        "_id" : "1",
        "_score" : 1.0,
        "_source" : {
          "username" : "dome_1",
          "age" : 1
        }
      }
    ]
  }
}
```

##### 批量查询文档
> 格式：GET /\_mget http-body
> es 允许查询多个文档

```
# request
GET /_mget
{
  "docs": [
    {
      "_index": "demo",
      "_type": "doc",
      "_id": "1"
    },
    {
      "_index": "demo",
      "_type": "doc",
      "_id": "2"
    }
  ]
}

# response
{
  "docs" : [
    {
      "_index" : "demo",
      "_type" : "doc",
      "_id" : "1",
      "found" : false
    },
    {
      "_index" : "demo",
      "_type" : "doc",
      "_id" : "2",
      "_version" : 2,
      "found" : true,
      "_source" : {
        "username" : "dome_2",
        "age" : 20
      }
    }
  ]
}
```
