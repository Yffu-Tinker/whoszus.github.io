```shell
#docker start shell 
docker run -it -d --net=host --privileged=true -v /home/files:/home/tinker/temp \
join --addr 127.0.0.1:2375
--restart=always --name tinker-files-server tinker-nginx:finish 

#开机启动docker服务 
systemctl enable docker.service
sysstemctl start  docker.service #启动docker服务

# 删除none的docker images 
docker rmi $(docker images | grep "none" | awk '{print $3}') 

# 批量删除容器
docker rm $(docker ps -qa)
docker ps -qa | xargs -n 1 docker rm
# 该脚本可以将 --registry-mirror 加入到你的 Docker 配置文件 /etc/docker/daemon.json 中
# 此处有个问题：  vim /etc/docker/daemon.json 需要删除这个文件里面最后一个逗号.....
curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://33fbba7b.m.daocloud.io

```

