# MongoDB

## 1. 单机部署

### 1.1 使用 compose 方式部署

```bash
# 部署命令
docker-compose -f mysql-8.0-single.yaml up -d

# 查看启动日志
docker-compose -f mysql-8.0-single.yaml logs -f
```