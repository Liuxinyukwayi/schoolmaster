# SchoolCMS Docker 快速启动指南

## 前置要求

- 已安装 Docker Desktop（Windows/Mac）或 Docker + Docker Compose（Linux）
- 确保端口 8080、3306、8081 未被占用

## 快速启动

### 1. 启动所有服务

```bash
docker-compose up -d
```

### 2. 查看服务状态

```bash
docker-compose ps
```

### 3. 查看日志

```bash
# 查看所有服务日志
docker-compose logs -f

# 查看特定服务日志
docker-compose logs -f web
docker-compose logs -f db
```

## 访问地址

- **前台首页**: http://localhost:8080
- **后台管理**: http://localhost:8080/admin.php
- **安装向导**: http://localhost:8080/Install/index.php
- **phpMyAdmin**: http://localhost:8081

## 安装步骤

1. 访问安装向导：http://localhost:8080/Install/index.php

2. 按照向导提示完成安装：
   - **数据库主机**: `db`（在容器内）或 `127.0.0.1`（从宿主机访问）
   - **数据库端口**: `3306`
   - **数据库名**: `schoolcms`（或自定义）
   - **数据库用户名**: `root`
   - **数据库密码**: `root`
   - **表前缀**: `sc_`（默认）

3. 安装完成后即可使用系统

## 数据库连接信息

### 从 Docker 容器内连接
- 主机: `db`
- 端口: `3306`
- 用户名: `root`
- 密码: `root`

### 从宿主机连接（使用数据库客户端）
- 主机: `127.0.0.1` 或 `localhost`
- 端口: `3306`
- 用户名: `root`
- 密码: `root`

## 常用命令

### 停止服务
```bash
docker-compose stop
```

### 停止并删除容器
```bash
docker-compose down
```

### 停止并删除容器和数据卷（⚠️ 会删除数据库数据）
```bash
docker-compose down -v
```

### 重新构建镜像
```bash
docker-compose build --no-cache
docker-compose up -d
```

### 进入容器
```bash
# 进入 PHP 容器
docker-compose exec web bash

# 进入 MySQL 容器
docker-compose exec db bash
```

### 执行 MySQL 命令
```bash
docker-compose exec db mysql -uroot -proot
```

## 目录权限

如果遇到文件写入权限问题，可以执行：

```bash
# Windows (Git Bash 或 WSL)
docker-compose exec web chown -R www-data:www-data /var/www/html
docker-compose exec web chmod -R 755 /var/www/html/Application/Runtime
docker-compose exec web chmod -R 755 /var/www/html/Application/Common/Conf
docker-compose exec web chmod -R 755 /var/www/html/Public/Upload
```

## 重新安装

如果需要重新安装系统：

1. 删除安装锁文件：
```bash
# Windows PowerShell
Remove-Item Install\install.lock -ErrorAction SilentlyContinue

# Linux/Mac
rm -f Install/install.lock
```

2. 清空数据库（可选）：
```bash
docker-compose exec db mysql -uroot -proot -e "DROP DATABASE IF EXISTS schoolcms; CREATE DATABASE schoolcms;"
```

3. 重新访问安装向导

## 故障排查

### 1. 端口被占用
如果端口被占用，可以修改 `docker-compose.yml` 中的端口映射：
```yaml
ports:
  - "8080:80"  # 改为其他端口，如 "8082:80"
```

### 2. 数据库连接失败
- 确保 `db` 服务已启动：`docker-compose ps`
- 检查数据库日志：`docker-compose logs db`
- 等待数据库完全启动（首次启动可能需要 30-60 秒）

### 3. 文件权限问题
参考上面的"目录权限"部分

### 4. PHP 扩展缺失
检查 Dockerfile 中是否包含所需扩展，然后重新构建：
```bash
docker-compose build --no-cache web
docker-compose up -d
```

## 服务说明

- **web**: PHP 7.4 + Apache 服务，运行 SchoolCMS 应用
- **db**: MySQL 5.7 数据库服务
- **phpmyadmin**: 数据库管理工具（可选）

## 技术栈

- PHP: 7.4
- Apache: 2.4
- MySQL: 5.7
- phpMyAdmin: Latest

