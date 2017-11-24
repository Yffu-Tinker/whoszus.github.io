	yum -y groupinstall development \
	&& yum -y install zlib-devel \
	&& yum -y install lua-devel \
	&& yum -y install pcre-devel openssl openssl-devel \
	&& yum -y install openssl-devel \
	&& yum -y install  gd-devel \
	&& export LUAJIT_LIB=/path/to/luajit/lib 
	&& export LUAJIT_INC=/path/to/luajit/include/luajit-2.0\
	&& mkdir -p /home/tinker/tools \ 
	&& cd /home/tinker/tools \ 
   	&& wget https://github.com/git/git/archive/v2.9.2.tar.gz \
    && tar -xzvf v2.9.2.tar.gz -C /home/tinker/tools \
    && cd git-2.9.2 \
    && make prefix=/usr/local/git all \
    && make prefix=/usr/local/git install \
	&& echo "export PATH=$PATH:/usr/local/git/bin" >> /etc/bashrc && source /etc/bashrc \
	&& cd /home/tinker/tools \ 
	&& git clone -b 2.2 https://github.com/vkholodkov/nginx-upload-module \ 
	&& git clone https://github.com/simpl/ngx_devel_kit.git \
	&& git clone https://github.com/openresty/lua-nginx-module.git \
	&& wget http://nginx.org/download/nginx-1.10.2.tar.gz  \
	&& tar -xvf nginx-1.10.2.tar.gz \
	&& cd  nginx-1.10.2 \
	&& ./configure --prefix=/opt/nginx \
         --with-ld-opt="-Wl,-rpath,/usr/local/luajit/lib" \
         --with-http_stub_status_module --with-http_ssl_module \
         --add-module=/home/tinker/tools/nginx-upload-module \
         --add-module=/home/tinker/tools/ngx_devel_kit \
         --add-module=/home/tinker/tools/lua-nginx-module \
         --with-http_image_filter_module \
   	&& make -j2 \
   	&& make install 