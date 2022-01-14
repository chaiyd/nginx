# nginx

## https://github.com/chaiyd/nginx.git
- chaiyd/nginx:tag
- tag会与nginx version尽量保持一致



## 说明
- 配置文件参考example

- http 
  - /etc/nginx/conf.d/
- stream
  - nginx tcp/udp 代理
  - /etc/nginx/stream

## docker-compose

```
version: '3'

services:

  nginx:
    container_name: nginx
    image: chaiyd/nginx:1.21.5
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
