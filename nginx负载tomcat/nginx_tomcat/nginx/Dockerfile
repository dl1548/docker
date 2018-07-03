FROM centos

MAINTAINER zili.li

ADD nginx-1.12.2.tar.gz /usr/local/src

RUN buildDeps='gcc gcc-c++ glib make autoconf openssl openssl-devel libxslt-devel gd gd-devel GeoIP GeoIP-devel pcre pcre-devel wget curl' \ 
                && yum -y install $buildDeps \
                && useradd -M -s /sbin/nologin nginx
#多个用逗号分隔
#docker inspect web_5 的 Mounts下可看到文件挂载信息.
#docker exec -it web_5 /bin/bash 进入容器可查看到挂载的目录,其内容和Mounts是同步的
VOLUME ["/usr/local/nginx/html"]

WORKDIR /usr/local/src/nginx-1.12.2

RUN ./configure --user=nginx --group=nginx --prefix=/usr/local/nginx --with-file-aio --with-http_ssl_module --with-http_realip_module --with-http_addition_module --with-http_xslt_module --with-http_image_filter_module --with-http_geoip_module --with-http_sub_module --with-http_dav_module --with-http_flv_module --with-http_mp4_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_auth_request_module --with-http_random_index_module --with-http_secure_link_module --with-http_degradation_module --with-http_stub_status_module \
                && make \
                && make install \
                && rm -rf /usr/local/src/nginx-1.12.2

#指定了环境变量,所以生成容器的时候 不用在指定路径.直接nginx 即可
ENV PATH /usr/local/nginx/sbin:$PATH                

EXPOSE 80

#当ENTRYPOINT和CMD连用时，CMD的命令是ENTRYPOINT命令的参数，两者连用相当于nginx -g "daemon off;"
#如果CMD ["-g","daemon on;"] 那么生成的容器将不会处于up状态.但是执行run的时候加入 -g "daemon off;"此参数将会传入给ENTRYPOINT
#容器中的应用都应该以前台执行,容器没有后台概念,-d 表示的后台,是程序的后台,程序完毕容器停止,而不是容器后台.容器都是前台的.
ENTRYPOINT ["nginx"]

#CMD service nginx start 这是初学经常搞模糊的地方!
#对于容器而言，其启动程序就是容器应用进程，容器就是为了主进程而存在的，主进程退出，容器就失去了存在的意义，从而退出.
# CMD service nginx start 会被理解为 CMD [ "sh", "-c", "service nginx start"]
#因此主进程实际上是 sh,那么当 service nginx start 命令结束后，sh 也就结束，sh作为主进程退出了自然就会令容器退出
#正确的做法是直接执行 nginx 可执行文件，并且要求以前台形式运行。比如：CMD ["nginx", "-g", "daemon off;"],或CMD /bin/sh -c 'nginx -g "daemon off;"'
CMD ["-g","daemon off;"]