---
layout: master
title:  使用Elasticsearch，创建/删除索引
description:  yuminglang.com,提供web前端、java、linux、数据库相关技术知识共享。
categories: Elasticsearch
syntax: 9
---
使用Elasticsearch，创建/删除索引,以及简单到索引列类型示例，使用cur命令操作，也可以将代码粘贴在Postman中使用。

##创建索引
```sh
curl -XPUT http:localhost:9200/es_test -d
'
{
 "setting": {
  "analysis": {
   "analyzer": {
    "cs_analyzer": {
     "tokenizer": "cs_tokenizer"
    }
   },
   "tokenizer": {
    "cs_tokenizer": {
     "type": "ngram",
     "min_gram": 1,
     "max_gram": 1,
     "token_chars": [
      "letter",
      "digit"
     ]
    }
   }
  }
 }
}
'
```

##指定索引中列数据类型,是否分词/存储
```sh
curl -XPOST http://localhost:9200/es_test/fulltext/_mapping -d
'
{
 "fulltext": {
  "properties": {
   "column_1": {
    "type": "long"
   },
   "column_2": {
    "type": "text",
    "fielddata": "true"
   },
   "column_3": {
    "type": "text"
   },
   "column_4": {
    "type": "text",
    "analyzer": "cs_analyzer",
    "search_analyzer": "cs_analyzer"
   },
   "column_5": {
    "type": "integer"
   },
   "column_6": {
    "type": "date"
   },
   "column_7": {
    "type": "keyword"
   }
  }
 }
'
```

##索引设置：查询最大返回量

```sh
curl -XPOST http://localhost:9200/es_test/_settings
'
{
	"index":{
		"max_result_window":10000000
	}
}
'
```

##删除索引

```sh
curl -XDELETE 'http://localhost:9200/es_test'
```

~         