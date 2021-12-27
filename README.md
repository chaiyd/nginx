# nginx

## https://github.com/chaiyd/nginx.git
- chaiyd/nginx:tagname
- tagname会与nginx version尽量保持一致



## 说明
- 配置文件参考conf.d
- 使用时挂载conf.d即可

## docker-compose

```
version: '3'

services:

  nginx:
    container_name: nginx
    image: chaiyd/nginx:1.20.2
    restart: always
    volumes:
      - ./data/logs:/etc/nginx/logs
      - ./data/conf.d:/etc/nginx/conf.d
      
    ports:
      - "80:80"
      - "443:443"
    #environment:
    #  - NGINX_PORT=80
    user: root

```
