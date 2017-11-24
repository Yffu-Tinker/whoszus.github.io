```shell

mkdir -p /home/tinker/temp/upload;
rm -rf /home/tinker/tools;
yum -y groupinstall development && yum -y install zlib-devel;
yum -y install lua-devel;
yum -y install pcre-devel openssl openssl-devel;
yum -y install openssl-devel;
yum -y install  gd-devel;
export LUAJIT_LIB=/opt/luajit/lib && export LUAJIT_INC=/opt/luajit/include/luajit-2.0;
mkdir -p /home/tinker/tools && cd /home/tinker/tools;
git clone -b 2.2 https://github.com/vkholodkov/nginx-upload-module && git clone https://github.com/simpl/ngx_devel_kit && git clone https://github.com/openresty/lua-nginx-module;
git clone https://github.com/openresty/lua-resty-upload.git;
wget http://nginx.org/download/nginx-1.10.2.tar.gz && tar -xvf nginx-1.10.2.tar.gz;
wget http://www.kyne.com.au/~mark/software/download/lua-cjson-2.1.0.tar.gz && tar -xvf lua-cjson-2.1.0.tar.gz;
cd /home/tinker/tools/lua-cjson-2.1.0;
make && make install PREFIX=/opt/luajit;
cd  /home/tinker/tools/nginx-1.10.2;
./configure --prefix=/opt/nginx \
--with-ld-opt="-Wl,-rpath,/opt/luajit/lib" \
--with-http_stub_status_module --with-http_ssl_module \
--add-module=/home/tinker/tools/nginx-upload-module \
--add-module=/home/tinker/tools/ngx_devel_kit \
--add-module=/home/tinker/tools/lua-nginx-module \
--with-http_image_filter_module ;
make -j2 && make install;
```

