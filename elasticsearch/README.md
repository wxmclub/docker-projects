# Elasticsearch

Elasticsearch 是一个分布式、RESTful 风格的搜索和数据分析引擎，能够解决不断涌现出的各种用例。

## 1. 单机部署

### 1.1 使用命令行方式部署

```bash
# 创建 Network: netes8
docker network create netes8

# elasticsearch
## 启动容器
docker run -d --name elasticsearch_8_15 --net netes8 -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e "xpack.security.enabled=false" -e _JAVA_OPTIONS="-XX:UseSVE=0" --restart=always wxmclub/elasticsearch-ext:8.15.5
## 进入容器
docker exec -it elasticsearch_8_15 bash

# kibana
## 启动容器
docker run -d --name kibana_8_15 --net netes8 -p 5601:5601 --link elasticsearch_8_15 -e "ELASTICSEARCH_HOSTS=http://elasticsearch_8_15:9200" --restart=always kibana:8.15.5
## 进入容器
docker exec -it kibana_8_15 bash
```

### 1.2 使用 compose 方式部署

```bash
# 部署命令
docker-compose -f es-8.15-single.yaml up -d
# 查看启动日志
docker-compose -f es-8.15-single.yaml logs -f
# 移除部署
docker-compose -f es-8.15-single.yaml down
```

## 2. 扩展

### 2.1 中文 ik 分词

```bash
# 进入容器
docker exec -it elasticsearch_8_15 /bin/bash

# 切换路径到plugins下
cd plugins
# 创建 ik 目录
mkdir ik
cd ik
# 下载文件，版本要与es一致
# https://github.com/infinilabs/analysis-ik
# https://release.infinilabs.com/
curl -O https://release.infinilabs.com/analysis-ik/stable/elasticsearch-analysis-ik-8.15.5.zip
# docker cp elasticsearch-analysis-ik-8.15.5.zip elasticsearch_8_15:/usr/share/elasticsearch/plugins/ik
# 等待下载完成后 解压
unzip elasticsearch-analysis-ik-8.15.5.zip
## 把解压文件elasticsearch下的全部内容移动到当前目录
#mv elasticsearch/* .
# 这时候就可以重启容器了 先退出
exit

# 重启容器
docker restart elasticsearch_8_15
```

- 测试结果

```
GET _analyze
{
  "analyzer": "ik_smart",
  "text": ["中文分词语"]
}
```

- 结果

```json
{
  "tokens": [
    {
      "token": "中文",
      "start_offset": 0,
      "end_offset": 2,
      "type": "CN_WORD",
      "position": 0
    },
    {
      "token": "分",
      "start_offset": 2,
      "end_offset": 3,
      "type": "CN_CHAR",
      "position": 1
    },
    {
      "token": "词语",
      "start_offset": 3,
      "end_offset": 5,
      "type": "CN_WORD",
      "position": 2
    }
  ]
}
```

### 2.2 关闭SSL认证

> es8.0之后默认开启了SSL认证，通过url:`http://localhost:9200/`访问时会报错:`received plaintext http traffic on an https channel, closing connection`

#### 2.2.1 改为通过`https`方式访问

#### 2.2.2 修改配置文件关闭SSL

* `/usr/share/elasticsearch/config/elasticsearch.yml`

```yaml
# 修改此配置为: false
xpack.security.enabled: false
```

```bash
sed -i 's/xpack.security.enabled: true/xpack.security.enabled: false/g' /usr/share/elasticsearch/config/elasticsearch.yml
```
