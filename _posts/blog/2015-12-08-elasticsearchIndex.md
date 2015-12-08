---
layout: post
title: elasticsearch文档索引及管理
description: elasticsearch索引以及映像的建立、搜索、配置等操作
category: blog
---

####1、建立索引

    curl -XPUT 'http://localhost:9200/mytest' -d '{
    "settings":{
      "index":{
     "number_of_shards":5,
     "number_of_replicas":1
    }  
    }
    }'
	
####使用*_settings*修改索引文件

    curl -XPUT 'http://localhost:9200/mytest/_settings' -d '{
      "index":{
     "number_of_replicas":7
    }
    }'
	
number_of_shards：设置分片数量
number_of_replicas：设置当前索引副本数量
block.read_only：若设置为true，当前索引只允许读，不允许写或者更新
block.read：若设置为true，禁止读取
block.write：若设置为true，禁止写
block.metadata：若设置为true，禁止对metadata进行操作

####获取索引较为详细的配置信息

    curl -XGET 'http://localhost:9200/mytest/_settings' 
	
获得多个索引的配置信息

    curl -XGET 'http://localhost:9200/mytest,dept/_settings'
	
获得全部索引的配置信息

     curl -XGET 'http://localhost:9200/_all/_settings'
	 
获取指定索引文件的状态信息

    curl -XGET 'http://localhost:9200/mytest/_status'
	
doc：被索引文档的信息，count描述索引中文档的数量
stores：索引的大小
indexing：索引操作信息
get：实时获取操作信息
search：搜索操作信息

####2、通过mapping映像配置索引

elasticsearch在使用时，不指定映像，会自动根据数据格式定义类型，但是如果需要对某些字段添加特殊属性（分词，存储等）就必须指定映像。

新建索引
   
    curl -XPUT 'http://localhost:9200/mytest1' -d '{
    "settings":{
     "number_of_shards":5,
     "number_of_replicas":1  
    },
    "mapping":{
        "wb":{}
    }
    }'

管理配置映像

     curl -XPUT 'http://localhost:9200/mytest1/wb/_mapping' -d '{
    "wb":{
        "properties":{
             "mymessage":{
                           "type":"string",
                           "store":true
                    }
         }
    }
    }'
	
获取映像信息

    curl -XGET 'http://localhost:9200/mytest1/_mapping/wb'
    curl -XGET 'http://localhost:9200/_all/_mapping/'
    curl -XGET 'http://localhost:9200/_mapping/'
    curl -XGET 'http://localhost:9200/mytest1/_mapping/wb,pages'
    curl -XGET 'http://localhost:9200/mytest1/_mapping/wb/field/mymessage'  //查看指定field的mapping
	
删除映像

    curl -XDELETE 'http://localhost:9200/mytest1/_mapping/wb'
    curl -XDELETE 'http://localhost:9200/mytest1/wb'
    curl -XDELETE 'http://localhost:9200/mytest1/wb/_mapping'

管理索引文件

一个关闭的索引将禁止写入或者读取对其中数据的操作，而打来索引可以允许用户对其中的数据文件进行操作

    curl -XPOST 'http://localhost:9200/mytest1/_open'
    curl -XPOST 'http://localhost:9200/mytest1/_close'
	
检测文件状态，返回200为存在，404为不存在
     
    curl -XHEAD 'http://localhost:9200/mytest1/' -v
	
删除索引

    curl -XDELETE 'http://localhost:9200/mytest1/'
	
清空索引缓存

    curl -XPOST 'http://localhost:9200/mytest1/_cache/clear'
    curl -XPOST 'http://localhost:9200/mytest1,mytest/_cache/clear'
刷新索引数据

    curl -XPOST 'http://localhost:9200/mytest1,mytest/_refresh'
    curl -XPOST 'http://localhost:9200/mytest1/_refresh'
    curl -XPOST 'http://localhost:9200/_refresh'
	
优化索引数据

    curl -XPOST 'http://localhost:9200/mytest1/_optimize'

将暂存于内存中的临时数据发送至索引文件，并清空内部操作日志

    curl -XPOST 'http://localhost:9200/mytest1/_flush'

####3、中文分词器

设置分词器需要先关闭当前索引，更改分词器之后再打开。

    curl -XPOST 'http://localhost:9200/mytest1/_close'
     curl -XPUT 'http://localhost:9200/mytest1/_settings' -d '{
      "analysis":{
     "analyzer":{
            "type":"custom",
            "tokenizer":"standard"
      }
    }
    }'
    curl -XPOST 'http://localhost:9200/mytest1/_open'
	
对指定的文件进行分词

    curl -XGET 'http://localhost:9200/_analyze?analyzer=standard&pretty' -d 'this is a test'

####4、对文档的其他操作

获取指定文档的信息

    curl -XGET 'http://localhost:9200/myfirstindex/share/1'
    curl -XGET 'http://localhost:9200/myfirstindex/share/1?pretty&_source=false'
    curl -XGET 'http://localhost:9200/myfirstindex/share/1?pretty&fields=location'

pretty表示返回的结果中显示缩进，以方便阅读
_source=false表示不显示source的内容
fields=location只显示location的内容

删除文档中的信息

    curl -XDELETE 'http://localhost:9200/myfirstindex/share/1'

数据更新

处理过程：先取出文档，运行指定脚本，然后再更新文档

    curl -XPUT 'http://localhost:9200/myfirstindex/share/2'  -d '{
       "url":"xtwang.github.io",
       "date":"2015-12-3",
       "location":"beijing"
     }'

    curl -XPOST 'http://localhost:9200/myfirstindex/share/2/_update' -d '{
          "script":"ctx._source.location += text",
           "params":{
                    "text":"test"
            }
    }'

创建文档并且加入新的字段

     curl -XPOST 'http://localhost:9200/myfirstindex/share/3/_update' -d '{
          "script":"ctx._source.location += text",
           "params":{
                    "text":"test"
            },
           "upsert":{
                   "counter":1
           }
     }'

当存在文档3时，使用script增加location字段的值，如果不存在，使用upsert增加文档，并且添加字段counter。

批量获取文档

    curl -XPOST 'http://localhost:9200/myfirstindex/share/_mget?' -d '{
         "docs":[
            {
                   "_location":"myfirstindex",
                   "_type":"share",
                   "_id":"1"
              },
             {
                   "_location":"myfirstindex",
                   "_type":"share",
                   "_id":"2"
              }
          ]
    }'

可以在？后面添加preety参数，获取其中一个文档的信息

删除部分文档

    curl -XDELETE 'http://localhost:9200/myfirstindex/share/_query?q=location:beijing'