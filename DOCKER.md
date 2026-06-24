# 远见在线考试系统 - Docker 一键启动

## 前置要求

- Docker >= 20.10
- Docker Compose >= 2.0

## 一键启动

```bash
# 1. 初始化数据库（首次启动需要）
cp yf_boot_exam.sql init-db.sql

# 2. 一键启动所有服务
docker compose up -d --build
```

等待约 1-2 分钟（MySQL 初始化 + 后端启动），然后访问：

| 服务 | 地址 |
|------|------|
| 前端页面 | http://localhost |
| 后端 API | http://localhost:8080 |
| Swagger 文档 | http://localhost:8080/doc.html |

## 默认账号

| 角色 | 用户名 | 密码 |
|------|--------|------|
| 管理员 | admin | admin |
| 学员 | student | student |

## 常用命令

```bash
# 查看所有容器状态
docker compose ps

# 查看后端日志
docker compose logs -f api

# 查看前端日志
docker compose logs -f web

# 停止所有服务
docker compose down

# 停止并删除数据卷（⚠️ 会清空数据库）
docker compose down -v

# 重启
docker compose restart
```

## 架构说明

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   Nginx      │     │   Spring     │     │    MySQL     │
│   (前端)     │────▶│   Boot       │────▶│    (3306)    │
│   :80        │     │   :8080      │     │              │
└──────────────┘     └──────┬───────┘     └──────────────┘
                            │
                            ▼
                     ┌──────────────┐
                     │    Redis     │
                     │    (6379)    │
                     └──────────────┘
```

- **前端**: Nginx 托管，反向代理 API 请求到 `api:8080`
- **后端**: Spring Boot 3.2 + Java 17，提供 REST API 和 Swagger 文档
- **数据库**: MySQL 8.0，自动执行 SQL 初始化
- **缓存**: Redis 7，用于会话管理和缓存

## 自定义配置

如需修改端口，编辑 `docker-compose.yml` 中的 `ports` 映射即可。
