# Coze Studio 简单部署说明

这份说明用于在本机或局域网内快速部署 Coze Studio。部署方式沿用官方 Docker Compose 方案。

## 环境要求

- 已安装并启动 Docker Desktop
- 已安装 Docker Compose
- 推荐机器配置不低于 2 Core / 4 GB 内存

## 拉取代码

```bash
git clone https://github.com/beyondchenlin/coze-studio.git
cd coze-studio
```

## 初始化环境文件

```bash
make web_env
```

该命令会从 `docker/.env.example` 生成 `docker/.env`。

## 开启局域网访问

编辑 `docker/.env`，找到：

```bash
export WEB_LISTEN_ADDR="127.0.0.1:8888"
```

改成：

```bash
export WEB_LISTEN_ADDR="0.0.0.0:8888"
```

这样同一局域网内的其他设备也可以访问服务。

## 启动服务

```bash
make web
```

第一次启动会拉取多个 Docker 镜像，时间会比较久。看到 `coze-web` 和 `coze-server` 都处于运行状态后即可访问。

## 访问地址

本机访问：

```text
http://127.0.0.1:8888/
```

局域网访问：

```text
http://你的局域网IP:8888/
```

macOS 可以用下面命令查看局域网 IP：

```bash
ipconfig getifaddr en0
```

例如返回 `192.168.1.127`，则局域网访问地址为：

```text
http://192.168.1.127:8888/
```

## 常用命令

查看容器状态：

```bash
docker compose -f docker/docker-compose.yml --env-file docker/.env ps
```

停止服务：

```bash
make down_web
```

重新启动：

```bash
make web
```

查看日志：

```bash
docker compose -f docker/docker-compose.yml --env-file docker/.env logs --tail=100
```

## 注意事项

- `docker/.env` 包含数据库密码、MinIO 密钥等本地配置，默认被 Git 忽略，不建议提交到仓库。
- 如果本机配置了 HTTP 代理，访问局域网 IP 时可能被代理拦截；可以在浏览器代理设置中排除局域网地址。
- 登录页语言取决于浏览器本地缓存。若需要强制中文，可在浏览器 Console 执行：

```js
localStorage.setItem('i18next', 'zh-CN');
location.reload();
```
