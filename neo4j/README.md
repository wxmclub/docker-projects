# Neo4j

## 1. 单机部署

* 访问地址: http://localhost:7474/
* 默认账号密码: neo4j/neo4j

### 1.1 使用 run 方式部署

```bash
#
docker run --name neo4j -p 7474:7474 -p 7687:7687 -v $HOME/neo4j/data:/data -d neo4j:5.25-community
```
