# GeeneAI 运维

# GeeneAI

## 万象

服务器：45.78.214.69

容器名称：pytorch2.5.1-cuda12.1-cudnn9-devel-shm

映射地址：/var/lib/docker/volumes/pytorch2.5.1-cuda12.1-cudnn9-devel-shm/_data/GeeneDify/Wan2.1

### 启动服务
````shell
docker exec -it pytorch2.5.1-cuda12.1-cudnn9-devel-shm bash

cd GeeneDify/Wan2.1/

pkill -f uvicorn

uvicorn fast-api:app --host 0.0.0.0 --port 7861 --workers 1 --reload
````
## ComfyUI

访问地址(http://45.78.214.68:27860/)

服务器：45.78.214.68

容器名称：comfyui-pytorch2.5.1-cuda12.1-cudnn9-devel-shm

映射地址：/var/lib/docker/volumes/pytorch2.5.1-cuda12.1-cudnn9-devel-shm/_data/GeeneDify/ComfyUI

上传文件的地址：/var/lib/docker/volumes/pytorch2.5.1-cuda12.1-cudnn9-devel-shm/_data/GeeneDify/ComfyUI/input

:::tip 注
通过万象的服务调用ComfyUI上传图片的接口,保存地址就是上面的上传文件地址
:::

生成图片额地址：/var/lib/docker/volumes/pytorch2.5.1-cuda12.1-cudnn9-devel-shm/_data/GeeneDify/ComfyUI/output

### 启动服务

````shell
docker exec -it comfyui-pytorch2.5.1-cuda12.1-cudnn9-devel-shm bash

cd GeeneDify/ComfyUI

ps -ef

kill -9 [id]

python main.py --port 7860 --listen 0.0.0.0
````



## dify

### 45.78.213.119  prod

数据库 45.78.213.117:5432  域名为：postgres63a4cbbfb6ca.rds-pg.ibytepluses.com  pgsql	root/GeeneAI@App2025  数据库为dify-geene-ai

redis redis-mlrfhiprlj6y9ccoz.redis.ibytepluses.com app/GeeneAIApp2025  用的是10库和11库

weaviate 45.78.213.119  2个集群(8080|8081)部署都在此服务器上  

访问端口是 80

访问后端端口是 5001

对外访问端口是 80

对外的后端端口 10.0.1.24:17900   10.0.1.25:17900

自身的nginx代理为
````shell
server {
    listen       7900;
    server_name  localhost;

    #charset koi8-r;
    #access_log  /var/log/nginx/host.access.log  main;

    location / {
       proxy_pass   http://10.0.1.24:5001;
    }
    
server {
    listen       3000;
    server_name  localhost;

    #charset koi8-r;
    #access_log  /var/log/nginx/host.access.log  main;

    location / {
       proxy_pass   http://10.0.1.24;
    }
````

### 45.78.213.124  prod
访问端口是 80

访问后端端口是 5001

对外访问端口是 80

对外的后端端口 10.0.1.24:17900   10.0.1.25:17900

自身的nginx代理为
````shell
server {
    listen       7900;
    server_name  localhost;

    #charset koi8-r;
    #access_log  /var/log/nginx/host.access.log  main;

    location / {
       proxy_pass   http://10.0.1.24:5001;
    }
    
server {
    listen       3000;
    server_name  localhost;

    #charset koi8-r;
    #access_log  /var/log/nginx/host.access.log  main;

    location / {
       proxy_pass   http://10.0.1.24;
    }
````

### 45.78.212.56  uat

基于docker compose部署 单台服务器部署  

### 45.78.214.72  self

基于docker compose部署 单台服务器部署

在此服务器上制作镜像


### 45.78.214.70  45.78.214.7

45.78.214.70  部署 数据库 redis  weaviate  全是默认middleware.env里的值 只有weaviate的端口改为 8090(8080冲突了)

45.78.214.7   部署业务服务  测试


## 升级

### 45.78.214.72 服务器做升级操作

git pull origin main

修改对应文件

修改 docker-compose.yaml .env 2个文件

docker build -f Dockerfile_zhangling_131 -t ifamilystory1/dify-api:1.3.1-zhangling-20250507-2 .

docker login -u ifamilystory@aliyun.com

Zhangling@2

docker push ifamilystory1/dify-api:1.3.1-zhangling-20250507-1

### 45.78.212.56 服务器做升级操作



cp .env ./backups/.env.$(date +%s).bak
cp docker-compose.yaml ./backups/docker-compose.yaml.$(date +%s).bak
rm -f docker-compose.yaml


git pull origin main
修改 docker-compose.yaml .env 2个文件

docker compose down

tar -cvf ./backups/volumes-$(date +%s).tgz volumes # 先down 再备份

docker compose up -d

### 445.78.214.70 服务器做升级第三方组件 

一般不用更新

cp docker-compose.middleware.yaml ./backups/docker-compose.middleware.yaml.$(date +%s).bak

rm -f docker-compose.middleware.yaml

git pull origin main

### 445.78.214.7 服务器做升级

cp .env ./backups/.env.$(date +%s).bak
cp docker-compose.yaml ./backups/docker-compose.yaml.$(date +%s).bak

git pull origin main

docker compose up -d

### 45.78.213.119 服务器做升级

cp .env ./backups/.env.$(date +%s).bak
cp docker-compose.yaml ./backups/docker-compose.yaml.$(date +%s).bak
rm -f docker-compose.yaml

git pull origin main
修改 docker-compose.yaml .env 2个文件

docker compose down

tar -cvf ./backups/volumes-$(date +%s).tgz volumes # 先down 再备份

docker compose up -d

### 45.78.213.124 服务器做升级

cp .env ./backups/.env.$(date +%s).bak
cp docker-compose.yaml ./backups/docker-compose.yaml.$(date +%s).bak
rm -f docker-compose.yaml

git pull origin main
修改 docker-compose.yaml .env 2个文件

docker compose down

tar -cvf ./backups/volumes-$(date +%s).tgz volumes # 先down 再备份

docker compose up -d

