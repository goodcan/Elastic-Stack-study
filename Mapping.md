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
