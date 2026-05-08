# vhr - 微人事 (人力资源管理系统)

微人事是一个前后端分离的人力资源管理系统，基于 **Spring Boot 2.4.0 + Vue 2 + ElementUI** 开发，整合了 Redis、RabbitMQ、WebSocket 等企业级技术栈。

> 本项目基于 [lenve/vhr](https://github.com/lenve/vhr) 二次开发，增加了 **Docker 一键部署** 支持。

## 项目技术栈

### 后端
Spring Boot · Spring Security · MyBatis · MySQL · Redis · RabbitMQ · Spring Cache · WebSocket

### 前端
Vue 2 · ElementUI · axios · vue-router · Vuex · WebSocket · vue-cli4

### 项目效果图

不同用户在登录后会根据角色看到不同的菜单：

![p278](https://raw.githubusercontent.com/wiki/lenve/vhr/doc/p278.png)

![p279](https://raw.githubusercontent.com/wiki/lenve/vhr/doc/p279.png)

系统管理员分配用户角色：

![p280](https://raw.githubusercontent.com/wiki/lenve/vhr/doc/p280.png)

管理角色可操作的资源：

![p281](https://raw.githubusercontent.com/wiki/lenve/vhr/doc/p281.png)

## Docker 一键部署（推荐）

项目根目录提供了完整的 Docker 编排文件，一条命令启动所有服务：

### 前置条件

- Docker (>= 20.10)
- Docker Compose (>= v2)

### 启动

```bash
# 克隆项目
git clone git@github.com:chudoga/vhr.git
cd vhr

# 启动所有服务
docker compose up -d
```

### 访问

| 服务 | 地址 |
|------|------|
| 前端页面 | [http://localhost:80](http://localhost) |
| 后端 API | [http://localhost:8081](http://localhost:8081) |
| RabbitMQ 管理 | [http://localhost:15672](http://localhost:15672) (guest/guest) |

### 服务构成

| 容器 | 端口映射 | 说明 |
|------|---------|------|
| `vhr-mysql` | 3307→3306 | 数据库（Root密码: `123`） |
| `vhr-redis` | 6380→6379 | 会话缓存（密码: `123`） |
| `vhr-rabbitmq` | 5673→5672, 15672 | 消息队列（guest/guest） |
| `vhr-backend` | 8081→8081 | Spring Boot 后端 |
| `vhr-frontend` | 80→80 | Nginx + Vue SPA |

### 默认账号

- **用户名**: `admin`
- **密码**: `123`

> 首次登录需要输入验证码。

### 停止

```bash
docker compose down
```

## 本地开发部署

如果不使用 Docker，也可以按传统方式运行：

### 后端

1. 创建 MySQL 数据库 `vhr`（脚本由 Flyway 自动管理）
2. 修改 `application.properties` 中的数据库、Redis、RabbitMQ 配置
3. 启动 `mailserver` 模块
4. 启动 `vhrserver/vhr-web` 模块

### 前端

```bash
cd vuehr
npm install
npm run serve
```

前端默认在 `http://localhost:8080` 启动，请求自动代理到后端 `8081` 端口。

## 相关文档

- [权限数据库设计](https://github.com/lenve/vhr/wiki/1.%E6%9D%83%E9%99%90%E6%95%B0%E6%8D%AE%E5%BA%93%E8%AE%BE%E8%AE%A1)
- [服务端环境搭建](https://github.com/lenve/vhr/wiki/2.%E6%9C%8D%E5%8A%A1%E7%AB%AF%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA)
- [更多 Wiki](https://github.com/lenve/vhr/wiki)

## License

```
Copyright 2018 王松 (lenve)

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
