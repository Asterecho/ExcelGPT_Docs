# 本地知识库

在ExcelGPT ProMax版中，将会增加**本地知识库**功能

到时候，生成Excel函数和VBA代码将会更加精确，有望实现直接操控表格

预期使用FastGPT的方案，敬请期待


## 安装docker-compose

```
mkdir -p /usr/local/bin
curl -L https://ghproxy.com/https://github.com/docker/compose/releases/download/v2.21.0/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose -k
chmod +x /usr/local/bin/docker-compose
# 验证安装
docker -v
docker-compose -v
```

## 部署FastGPT

```
mkdir fastgpt
cd fastgpt
touch docker-compose.yml
```

```
# 非 host 版本, 不使用本机代理
# 非 host 版本, 不使用本机代理
version: '3.3'
services:
  pg:
    #image: ankane/pgvector:v0.4.2 # git
    image: registry.cn-hangzhou.aliyuncs.com/fastgpt/pgvector:v0.4.2 # 阿里云
    container_name: pg
    restart: always
    ports: # 生产环境建议不要暴露
      - 5432:5432
    networks:
      - fastgpt
    environment:
      # 这里的配置只有首次运行生效。修改后，重启镜像是不会生效的。需要把持久化数据删除再重启，才有效果
      - POSTGRES_USER=username
      - POSTGRES_PASSWORD=password
      - POSTGRES_DB=postgres
    volumes:
      - ./pg/data:/var/lib/postgresql/data
  mongo:
    #image: mongo:5.0.18
    image: registry.cn-hangzhou.aliyuncs.com/fastgpt/mongo:5.0.18 # 阿里云
    container_name: mongo
    restart: always
    ports: # 生产环境建议不要暴露
      - 27017:27017
    networks:
      - fastgpt
    environment:
      # 这里的配置只有首次运行生效。修改后，重启镜像是不会生效的。需要把持久化数据删除再重启，才有效果
      - MONGO_INITDB_ROOT_USERNAME=username
      - MONGO_INITDB_ROOT_PASSWORD=password
    volumes:
      - ./mongo/data:/data/db
  fastgpt:
    container_name: fastgpt
    # image: c121914yu/fast-gpt:latest # docker hub
    image: registry.cn-hangzhou.aliyuncs.com/fastgpt/fastgpt:latest # 阿里云
    ports:
      - 3000:3000
    networks:
      - fastgpt
    depends_on:
      - mongo
      - pg
    restart: always
    environment:
      # root 密码，用户名为: root
      - DEFAULT_ROOT_PSW=1234
      # 中转地址，如果是用官方号，不需要管
      - OPENAI_BASE_URL=https://openai.api2d.net/v1
      - CHAT_API_KEY=fk186214-zJDvkFCBInfcc6p9VBQfF6rq7iGSAYsm|ck265-cddc746
      - DB_MAX_LINK=5 # database max link
      - TOKEN_KEY=any
      - ROOT_KEY=root_key
      # mongo 配置，不需要改
      - MONGODB_URI=mongodb://username:password@mongo:27017/?authSource=admin
      - MONGODB_NAME=fastgpt
      # pg配置.
      - PG_HOST=pg
      - PG_PORT=5432
      - PG_USER=username
      - PG_PASSWORD=password
      - PG_DB_NAME=postgres

networks:
  fastgpt:
```

## 启动容器

```
# 在 docker-compose.yml 同级目录下执行
docker-compose up -d
```