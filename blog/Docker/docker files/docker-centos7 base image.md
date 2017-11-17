```

```

```
FROM centos:7
ENV container docker
RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == \
systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;
VOLUME [ "/sys/fs/cgroup" ]
CMD ["/usr/sbin/init"]
```

This Dockerfile deletes a number of unit files which might cause issues. From here, you are ready to build your base image.

```
$ docker build --rm -t local/c7-systemd 
```

In order to use the systemd enabled base container created above, you will need to create your `Dockerfile`similar to the one below.

```
FROM local/c7-systemd
RUN (rm -rf /home/tinker/tools;
yum -y groupinstall development && yum -y install zlib-devel;
yum -y install lua-devel;
yum -y install pcre-devel openssl openssl-devel;
yum -y install openssl-devel;
yum -y install  gd-devel;
export LUAJIT_LIB=/opt/luajit/lib && export LUAJIT_INC=/opt/luajit/include/luajit-2.0;
mkdir -p /home/tinker/tools && cd /home/tinker/tools;
git clone -b 2.2 https://github.com/vkholodkov/nginx-upload-module;
git clone https://github.com/simpl/ngx_devel_kit && git clone https://github.com/openresty/lua-nginx-module;
git clone https://github.com/openresty/lua-resty-upload.git;
wget http://nginx.org/download/nginx-1.10.2.tar.gz && tar -xvf nginx-1.10.2.tar.gz;
wget http://luajit.org/download/LuaJIT-2.0.5.tar.gz && tar -xvf LuaJIT-2.0.5.tar.gz;
cd /home/tinker/tools/LuaJIT-2.0.5 && make  && make install PREFIX=/opt/luajit ;
cd  /home/tinker/tools/nginx-1.10.2;
./configure --prefix=/opt/nginx \
--with-ld-opt="-Wl,-rpath,/opt/luajit/lib" \
--with-http_stub_status_module --with-http_ssl_module \
--add-module=/home/tinker/tools/nginx-upload-module \
--add-module=/home/tinker/tools/ngx_devel_kit \
--add-module=/home/tinker/tools/lua-nginx-module \
--with-http_image_filter_module ;
make -j2 && make install;
)
EXPOSE 80
CMD ["/usr/sbin/init"]
```

Build this image:

```
$ docker build --rm -t local/c7-systemd-httpd 
```