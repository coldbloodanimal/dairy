[第一步安装与官方文档](https://mp.csdn.net/postedit/95592237)

[官方视频网址](https://www.elastic.co/cn/webinars/getting-started-elasticsearch?baymax=default&elektra=docs&storm=top-video)
![图1](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html)
kibana 可视化&管理
elasticsearch 存储，搜索& 分析
logstash 摄取
beats 轻量级 摄取

练习的时候把elasticsearch和kibana(国内docker慢的原因，安装包按了)先安装了

[kibana本地开发工具访问地址](http://localhost:5601/app/kibana#/dev_tools/console?_g=())
增删改查
结构含义 index/type/id
type据说已经不需要了，但测试的时候出问题了，id感觉也可以不要自动生成

POST ice/_doc/2

{
  "name":"xiaohong",
  "age":10
}

DELETE ice/_doc/2

PUT ice/_doc/2

{
  "name":"xiaohonghong",
  "age":10
}

GET ice/_doc/2

复杂查询 条件查询 查询名称为小红的
GET ice/_search

{
  "query": {
    "match": {
      "name": "xiaohong"
    }
  }
}

复杂查询 与 bool是要最后里面的所有判断提交为真，查询name为小红，且年龄10岁的
GET ice/_search

{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "name": "xiaohong"
          }
        },
        {
          "match": {
            "age": "10"
          }
        }
      ]
    }
  }
}

复杂查询 非 must_not名字是小红，年龄不是10岁的
GET ice/_search

{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "name": "xiaohong"
          }
        }
      ],"must_not": [
        {"match": {
          "age": "10"
        }}
      ]
    }
  }
}

复杂查询 或 should
GET ice/_search

{
  "query": {
    "bool": {
      "should": [
        {
          "match": {
            "name": "xiaohong"
          }
        },{
                    "match": {
            "name": "xiaoming"
          }
        }
      ]
    }
  }
}

聚合查询 _count 
GET ice/_count

{
  "query": {
    "bool": {
      "should": [
        {
          "match": {
            "name": "xiaohong"
          }
        },
        {
          "match": {
            "name": "xiaoming"
          }
        }
      ]
    }
  }
}

