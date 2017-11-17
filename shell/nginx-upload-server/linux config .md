# 安装python 


# yum -y groupinstall development
# yum -y install zlib-devel

# wget https://www.python.org/ftp/python/3.6.0/Python-3.6.0.tar.xz
# tar xJf Python-3.6.0.tar.xz
# cd Python-3.6.0
# ./configure
# make && make install

# 安装nginx 

# rpm -ivh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
# yum install nginx
# service nginx start

# http://www.centoscn.com/nginx/2017/0119/8422.html


# 分布式nginx方案


# 卸载nginx 
# yum remove nginx 
# wget http://nginx.org/download/nginx-1.10.2.tar.gz  
# git clone -b 2.2 https://github.com/vkholodkov/nginx-upload-module  

# yum install lua-devel -y

# yum -y install pcre-devel openssl openssl-devel 

# yum install openssl-devel
# yum install -y gd-devel


 # tell nginx's build system where to find LuaJIT 2.0:
 export LUAJIT_LIB=/path/to/luajit/lib
 export LUAJIT_INC=/path/to/luajit/include/luajit-2.0


 # Here we assume Nginx is to be installed under /opt/nginx/.
 ./configure --prefix=/opt/nginx \
         --with-ld-opt="-Wl,-rpath,/usr/local/luajit/lib" \
         --with-http_stub_status_module --with-http_ssl_module \
         --add-module=/home/tinker/tools/nginx-upload-module \
         --add-module=/home/tinker/tools/ngx_devel_kit \
         --add-module=/home/tinker/tools/lua-nginx-module \
          --with-http_image_filter_module




make -j2
make install