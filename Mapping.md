## Mapping
### 介绍
类似数据库中的表结构定义

### 主要作用
- 定义 Index 下的字段名（Field Name）
- 定义字段的类型，比如数值型，字符串型、布尔型等
- 定义倒排索引相关的配置，比如是否索引、记录 position 等

### api
#### 查看索引 mapping
> 格式：GET /index-name/\_mapping

```
# request
GET /demo/_mapping

# response
{
  "demo" : {
    "mappings" : {
      "doc" : {
        "properties" : {
          "age" : {
            "type" : "long"
          },
          "usernae" : {
            "type" : "text",
            "fields" : {
              "keyword" : {
                "type" : "keyword",
                "ignore_above" : 256
              }
            }
          },
          "username" : {
            "type" : "text",
            "fields" : {
              "keyword" : {
                "type" : "keyword",
                "ignore_above" : 256
              }
            }
          }
        }
      }
    }
  }
}
```

### 自定义 Mapping
#### API

```
# request
# mapping 通过这个关键字进行配置
# type 字段类型
PUT /demo
{
  "mappings": {
    "doc": {
      "properties": {
        "title": {
          "type": "text"
        },
        "name": {
          "type": "keyword"
        },
        "age": {
          "type": "integer"
        }
      }
    }
  }
}

# response
{
  "acknowledged" : true,
  "shards_acknowledged" : true,
  "index" : "demo"
}
```

#### 注意项
- Mapping 中的字段类型一旦设定后，禁止直接修改
    - Lucene 实现的倒排索引生成后不允许修改
- 重新建立新的索引，然后做 reindex 操作

#### 新增字段
> 通过 dynamic 参数来控制字段的新增

- true（默认）允许自动新增字段
- fase 不允许自动新增字段，但是文档可以正常写入，但无法对字段进行查询等操作
- strict 文档不能写入，报错

```
# request
PUT /demo
{
  "mappings": {
    "my_type": {
      "dynamic": false,
      "properties": {
        "user": {
          "properties": {
            "name": {
              "type": "text"
            },
            "social_networks": {
              "dynamic": true,
              "properties": {}
            }
          }
        }
      }
    }
  }
}

# response
{
  "acknowledged" : true,
  "shards_acknowledged" : true,
  "index" : "demo"
}
```
