[官方地址](https://www.elastic.co/guide/en/elasticsearch/reference/current/geo-point.html)

设置map，类似于设计表结构
    PUT my_index
    {
      "mappings": {
        "properties": {
          "location": {
            "type": "geo_point"
          }
        }
      }
    }
    
使用4中不同形式插入地理数据

    PUT my_index/_doc/1
    {
      "text": "Geo-point as an object",
      "location": { 
        "lat": 41.12,
        "lon": -71.34
      }
    }

    PUT my_index/_doc/2
    {
      "text": "Geo-point as a string",
      "location": "41.12,-71.34" 
    }

    PUT my_index/_doc/3
    {
      "text": "Geo-point as a geohash",
      "location": "drm3btev3e86" 
    }

    PUT my_index/_doc/4
    {
      "text": "Geo-point as an array",
      "location": [ -71.34, 41.12 ] 
    }

地理信息查询

