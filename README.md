# 安装Docker Engine on ubuntu
https://docs.docker.com/install/linux/docker-ce/ubuntu/

Update your apt sources 更新apt 源
apt安装docker
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io

## 配置为无需sudo
run docker with non-root user       sudo
https://docs.docker.com/install/linux/linux-postinstall/
To create the docker group and add your user:
1.Create the docker group.

$sudo groupadd docker
2.Add your user to the docker group.
$sudo usermod -aG docker $USER
3.Log out and log back in so that your group membership is re-evaluated.

# 运行
## 1 启动docker daemon 
ubuntu: 
sudo service docker start
CentOS:
sudo systemctl start docker
## 2 docker image 管理
查看所有images
docker images
删除docker image
docker rmi  imagename
从Dockerhub拉取一个image
docker pull  xxx

## 3 启动docker container
sudo docker run hello-world
以root 启动的docker daemon ， 启动docker image 也需root 
启动一个ubuntu docker ，并进入bash
sudo docker run -it ubuntu bash
直接启动docker image , 同上线状态
docker run -it <your-image>
nvidia runtime
docker run --runtime=nvidia -it <image>
挂载 host dir
docker run -v  /host/dir:/container/dir <image>
端口映射
docker run -p hostport:container_port <image>
设置环境变量
docker run -e RACV_DATA_PART=3  <image>
设置work_dir
docker run -w  /work_dir
### 3.1 container 查看/删除
查看所有container
docker ps -a
docker ps -a -q  ## 只看containerID
删除docker container
    docker rm  container_name
docker rm $(docker ps -a -q)
## 4 查看container log
docker container logs  containerID
## 5 进入已经启动的docker container
docker exec -it containID  /bin/bash
## 6 stop container
docker container  stop containID
docker container  rm containID
debug container 
https://medium.com/@betz.mark/ten-tips-for-debugging-docker-containers-cde4da841a1d
docker logs 
exec 只能在container running时可用，若container启动就崩溃，无法使用
build docker image 
https://docs.docker.com/engine/getstarted/step_four/

# 编写Dockerfile
https://docs.docker.com/engine/reference/builder/
官方image:
FROM library/ubuntu:14.04.4                                      ##官网环境，从 docker.io 下载原始image
FROM nvidia/cuda:9.0-cudnn7-devel-ubuntu16.04   ##
qiyi 写法：
FROM docker-registry.qiyi.virtual/library/centos7:1.4
FROM docker-registry.qiyi.virtual/docker/ubuntu:14.04.3
注：
查看有哪些可用image
http://dr.qiyi.virtual/repositories/20
CMD
多条命令
CMD /etc/init.d/nullmailer start ; /usr/sbin/php5-fpm
ENV
设置进入bash的环境变量

RUN
https://docs.docker.com/engine/reference/run/
docker run -e "RACV_DATA_PART=3"   ## run with env  启动时指定环境变量
build docker 
docker build -t nginx_image PATH
PATH is required,  context path, for ADD or COPY command
-t  target name
-f dockerfile  name
sudo docker build -t robotarm:0.2 . -f robotarm_algorithms/Dockerfiles/Dockerfile

# push docker image 到registry
login
docker login  dr.qiyi.virtual
sudo docker login reg.ainirobot.com   ## 有的登陆失败是由于没有用sudo
logout
docker logout  dr.qiyi.virtual
在项目中标记镜像：
sudo docker tag maskrcnn-benchmark:0.2 reg.ainirobot.com/ocr/maskrcnn-benchmark:0.2
同样的image 会多出一条记录：
REPOSITORY                                                TAG                            IMAGE ID               CREATED             SIZE
maskrcnn-benchmark                                     0.2                            edfb8cb7c2a1        12 minutes ago      5.85GB
reg.ainirobot.com/ocr/maskrcnn-benchmark 0.2                            edfb8cb7c2a1        12 minutes ago      5.85GB
推送镜像到当前项目：
sudo docker push  reg.ainirobot.com/ocr/maskrcnn-benchmark:0.2
Docker Hub  -- share docker image
https://hub.docker.com/

