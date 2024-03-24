# Redis

## 1. 单机部署

### 1.1 使用命令行方式部署

```bash
## 启动容器
docker run --name redis_7 -p 6379:6379 --restart=always -d redis:7-alpine
## 进入容器
docker exec -it redis_7 sh
## 在容器内进入控制台
redis-cli -h 127.0.0.1 -p 6379

# redis-cli 模式，如果是连接本机docker的redis服务，需要在同一个network下
docker run -it --rm redis:7-alpine redis-cli -h host -p 6379
```

### 1.2 使用 compose 方式部署

```bash
# 部署命令
docker-compose -f redis-7-single.yaml up -d
# 查看启动日志
docker-compose -f redis-7-single.yaml logs -f
# 移除部署
docker-compose -f redis-7-single.yaml down
```
