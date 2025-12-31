# Docker 配置文件说明

## 文件结构

- `docker/php/php.ini` - PHP 配置文件
- `docker/php/www.conf` - PHP-FPM 配置文件
- `docker/nginx/nginx.conf` - Nginx 主配置文件
- `docker/nginx/default.conf` - Nginx 虚拟主机配置
- `docker/mysql/my.cnf` - MySQL 配置文件

## 使用方法

1. 确保已安装 Docker 和 Docker Compose

2. 启动服务：
```bash
docker-compose up -d
```

3. 访问应用：
   - 前台：http://localhost:8080
   - 后台：http://localhost:8080/admin.php
   - 安装向导：http://localhost:8080/Install/index.php
   - phpMyAdmin：http://localhost:8081

4. 数据库连接信息（安装时使用）：
   - 数据库主机：`db`（在 Docker 容器内）或 `127.0.0.1`（从宿主机）
   - 数据库端口：`3306`
   - 数据库名：`schoolcms`（或自定义）
   - 用户名：`root`
   - 密码：`root`

5. 停止服务：
```bash
docker-compose down
```

6. 查看日志：
```bash
docker-compose logs -f
```

## 注意事项

- 首次安装需要通过安装向导完成数据库配置
- 确保 `Application/Runtime`、`Application/Common/Conf`、`Public/Upload` 等目录有写权限
- 如需重新安装，删除 `Install/install.lock` 文件
