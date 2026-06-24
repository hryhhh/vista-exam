# 远见在线考试系统

> 基于 Spring Boot 3 + Vue 3 的前后端分离在线考试系统，支持管理端与学员端双端操作，提供完整的考试流程与题库管理能力。

![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.2.1-red)
![Java](https://img.shields.io/badge/Java-17-blue)
![Vue](https://img.shields.io/badge/Vue-3.3-yellow)
![License](https://img.shields.io/badge/license-MIT-green)

---

## 目录

- [项目简介](#项目简介)
- [技术栈](#技术栈)
- [系统架构](#系统架构)
- [功能特性](#功能特性)
- [快速开始](#快速开始)
  - [Docker 一键启动（推荐）](#docker-一键启动推荐)
  - [本地开发环境](#本地开发环境)
- [系统截图](#系统截图)
- [默认账号](#默认账号)
- [常见问题](#常见问题)
- [联系方式](#联系方式)

---

## 项目简介

远见在线考试系统是一套功能完善的在线考试管理平台，涵盖从题库建设、智能组卷、在线答题到成绩分析的完整考试流程。系统采用前后端分离架构，支持 RBAC 权限管理、多题型支持、实时考试计时等功能，适用于学校培训、企业考核等多种场景。

---

## 技术栈

### 后端

| 技术 | 版本 | 用途 |
|------|------|------|
| Java | 17 | 开发语言 |
| Spring Boot | 3.2.1 | 应用框架 |
| MyBatis-Plus | 3.5.11 | ORM 框架 |
| Apache Shiro | 2.0.2 | 权限认证 |
| Spring Data Redis | - | 缓存 / 会话管理 |
| Quartz | - | 定时任务调度 |
| SpringDoc + Knife4j | 2.3.0 / 4.5.0 | API 文档 |
| Apache POI | 4.1.2 | Office 文件处理 |
| Hutool | 5.8.26 | 工具类库 |
| HikariCP | - | 数据库连接池 |

### 前端

| 技术 | 版本 | 用途 |
|------|------|------|
| Vue | 3.3.4 | 渐进式框架 |
| TypeScript | 5.1.6 | 类型安全 |
| Element Plus | 2.3.12 | UI 组件库 |
| Vite | 4.4.9 | 构建工具 |
| Pinia | 2.1.6 | 状态管理 |
| Vue Router | 4.2.4 | 路由管理 |
| Axios | 1.4.0 | HTTP 客户端 |
| ECharts | 5.4.3 | 数据可视化 |
| UnoCSS | 0.55.0 | 原子化 CSS |
| WangEditor | 5.x | 富文本编辑器 |

### 基础设施

| 组件 | 版本 | 用途 |
|------|------|------|
| MySQL | 8.0 | 关系型数据库 |
| Redis | 7 | 缓存与会话存储 |
| Nginx | alpine | 静态资源托管 / 反向代理 |

---

## 系统架构

```
┌─────────────────────────────────────────────────────────────┐
│                       浏览器                                │
└───────────────┬───────────────────────────────┬─────────────┘
                │                               │
           :80 HTTP                        :8080 API
                │                               │
┌───────────────▼──────────┐   ┌───────────────▼─────────────┐
│      Nginx (Web)         │   │    Spring Boot (API)        │
│  静态资源 + 反向代理      │──▶│  RESTful API + 业务逻辑      │
└──────────────────────────┘   └───────────┬─────────────────┘
                                           │
                        ┌──────────────────┼──────────────────┐
                        │                  │                   │
                   ┌────▼────┐      ┌──────▼──────┐    ┌──────▼──────┐
                   │ MySQL   │      │    Redis     │    │   Quartz   │
                   │ :3306   │      │   :6379      │    │ (JDBC)     │
                   └─────────┘      └──────────────┘    └─────────────┘
```

---

## 功能特性

### 管理端

| 模块 | 功能说明 |
|------|----------|
| 菜单管理 | 功能菜单及权限控制，通过角色权限动态加载前端路由 |
| 角色管理 | 定义角色关联的菜单及功能，实现动态权限管理 |
| 数据字典 | 支持多级树结构的分类字典与数据字典 |
| 个性配置 | 网站名称、登录图标、后台图标、登录踢出机制、注册开关等 |
| 插件管理 | 可扩展的一键集成插件机制（当前支持本地上传） |
| 部门管理 | 支持多级树结构的部门信息维护 |
| 人员管理 | 管理员与学员用户的统一维护 |
| 考试管理 | 考试信息维护、出题组卷策略配置、考试记录查看 |
| 题库管理 | 题库基本信息管理、题库统计概览 |
| 试题管理 | 支持单选题、多选题、判断题、不定项选择题的增删改查 |

### 学员端

| 模块 | 功能说明 |
|------|----------|
| 在线考试 | 考试列表浏览、在线答题、实时计时、自动交卷 |
| 考试记录 | 考试成绩查看、通过情况统计、考试明细回溯 |
| 试卷详情 | 查看试卷内容及各题答题情况 |
| 个人资料 | 个人信息维护、头像上传 |
| 密码安全 | 密码修改 |

---

## 快速开始

### Docker 一键启动（推荐）

**前置要求**：Docker >= 20.10，Docker Compose >= 2.0

```bash
# 1. 克隆项目
git clone <your-repo-url>
cd vista-online-exam

# 2. 一键启动（首次会自动构建镜像，约 3-5 分钟）
docker compose up -d --build

# 3. 等待 1-2 分钟后访问
# 前端页面：http://localhost
# 后端 API：http://localhost:8080
# Swagger 文档：http://localhost:8080/doc.html
```

**常用命令**：

```bash
docker compose ps              # 查看容器状态
docker compose logs -f api     # 查看后端日志
docker compose down            # 停止所有服务
docker compose down -v         # 停止并删除数据卷（⚠️ 会清空数据库）
```

修改代码后重新构建：

```bash
docker compose down -v
docker compose build --no-cache
docker compose up -d
```

### 本地开发环境

#### 环境要求

- JDK 17（推荐 Zulu JDK）
- MySQL 5.7 / 8.0
- Redis 4.x+
- Node.js >= 14.18（推荐 20+）
- pnpm（`npm install -g pnpm`）

#### 后端启动

```bash
# 1. 导入数据库
mysql -u root -p < yf_boot_exam.sql

# 2. 修改后端配置（yf-bev2-api/src/main/resources/application.yml）
#    将数据库和 Redis 地址改为 localhost

# 3. 用 IDEA 打开 yf-bev2-api 模块，运行 BootExamApplication

# 4. 访问 http://localhost:8080
```

#### 前端启动

```bash
cd yf-bev2-vue
pnpm install          # 安装依赖
pnpm run dev          # 启动开发服务器（端口 8000）
pnpm run build:pro    # 生产构建
```

> **快捷模式**：前端已预编译并集成到后端 `resources/static` 目录中，仅启动后端即可同时访问前后端。如需前后端分离部署，删除 `resources/static` 目录即可。

---

## 系统截图

> （待补充截图）

---

## 默认账号

| 角色 | 用户名 | 密码 | 说明 |
|------|--------|------|------|
| 管理员 | admin | admin | 拥有全部权限 |
| 学员 | student | student | 仅考试相关权限 |

---

## 常见问题

### 1. 后端启动时报 `QRTZ_TRIGGERS doesn't exist`

MySQL 表名大小写问题。Docker 部署时已自动处理（`lower-case-table-names=1`），本地部署请确保 MySQL 配置 `lower_case_table_names=1`。

### 2. 前端访问 404 或白屏

检查后端是否正常启动，浏览器控制台是否有 API 请求报错。确认 `SPRING_DATASOURCE_URL` 等环境变量配置正确。

### 3. 修改代码后如何生效？

Docker 部署：`docker compose down -v && docker compose build --no-cache && docker compose up -d`

本地开发：后端热重载（Spring Boot DevTools）或重启；前端 `pnpm run dev` 自动热更新。


## License

[MIT License](./LICENSE)
