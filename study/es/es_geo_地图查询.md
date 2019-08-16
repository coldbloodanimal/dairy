[官方地址](https://www.elastic.co/guide/en/elasticsearch/reference/current/geo-point.html)
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
  
