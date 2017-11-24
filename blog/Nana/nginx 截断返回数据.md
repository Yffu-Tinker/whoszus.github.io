#### nginx 截断返回数据



```
# 当前服务配置 ，配置
fastcgi_buffers 8 64K; 
fastcgi_buffer_size 256K;
```



排查发现是nginx所在服务器磁盘空间满了，缓存写不进去，导致截断了

```tex
# 排查过程：
	-1. 修改 fastcgi_buffers fastcgi_buffer_size 大小，然后重启nginx；
	-2. 修改nginx运行用户组为root
	-3. 排查磁盘空间
```



