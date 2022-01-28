FROM alpine:3.13 AS builder

LABEL maintainer="NGINX Docker Maintainers <chaiyd.cn@gmail.com>"

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories \
    && apk update  \
    && apk add --no-cache ca-certificates curl bash tree tzdata \
    && cp -rf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "Asia/Shanghai" > /etc/timezone \
    && apk add --no-cache \
                 pcre-dev \
	             gd-dev \
        gcc \
        libc-dev \
        make \
        openssl-dev \
        zlib-dev \
        linux-headers \
        curl \
        gnupg \
        libxslt-dev \
        geoip-dev \
        wget

ARG NGINX_VERSION=1.21.6
ARG CONFIG=" \
        --prefix=/etc/nginx \
        --with-http_ssl_module \
        --with-http_realip_module \
        --with-http_addition_module \
        --with-http_sub_module \
        --with-http_dav_module \
        --with-http_flv_module \
        --with-http_mp4_module \
        --with-http_gunzip_module \
        --with-http_gzip_static_module \
        --with-http_random_index_module \
        --with-http_secure_link_module \
        --with-http_stub_status_module \
        --with-http_auth_request_module \
        --with-http_xslt_module=dynamic \
        --with-http_geoip_module=dynamic \
        --with-threads \
        --with-stream \
        --with-stream_ssl_module \
        --with-stream_ssl_preread_module \
        --with-stream_realip_module \
        --with-stream_geoip_module=dynamic \
        --with-http_slice_module \
        --with-mail \
        --with-mail_ssl_module \
        --with-compat \
        --with-file-aio \
        --with-http_v2_module \
        --with-http_image_filter_module"

RUN addgroup -S nginx \
    && wget -c -P /tmp/ http://nginx.org/download/nginx-$NGINX_VERSION.tar.gz \
    && cd /tmp/ && tar -zxvf nginx-$NGINX_VERSION.tar.gz \
    && cd /tmp/nginx-$NGINX_VERSION \
    && ./configure $CONFIG \
    && make && make install
	
COPY conf/nginx.conf /etc/nginx/conf/nginx.conf
#COPY conf.d /etc/nginx/conf.d

FROM alpine:3.13

LABEL maintainer="NGINX Docker Maintainers <chaiyd.cn@gmail.com>"

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories \
    && apk update  \
    && apk add --no-cache ca-certificates curl bash tree tzdata \
    && cp -rf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "Asia/Shanghai" > /etc/timezone \
    && apk add --no-cache \
                 pcre-dev \
	             gd-dev

COPY --from=builder /etc/nginx/ /etc/nginx/
RUN ln -s /etc/nginx/sbin/nginx /usr/sbin/nginx

EXPOSE 80
EXPOSE 443

STOPSIGNAL SIGTERM

CMD ["nginx", "-g", "daemon off;"]
