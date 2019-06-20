# Docker Tutorials

## 1. 安装Docker Engine
https://docs.docker.com/install/linux/docker-ce/ubuntu/

#### Ubuntu
Update your apt sources 更新apt源
```
sudo apt-get update
```
apt安装docker
```
sudo apt-get install docker-ce docker-ce-cli containerd.io
```



## 2. 运行
### 2.1 启动docker daemon 
Ubuntu:
```
$ sudo service docker start
```
CentOS:
```
$ sudo systemctl start docker
```

### 2.2 docker image 管理
查看所有images
```
docker images
```
删除docker image
```
docker rmi  <image>   ## image name or id
```
从Dockerhub或其他registry拉取一个image
```
docker pull  <image>  
docker pull  ubuntu:16.04
```
### 2.3 启动docker container
```
sudo docker run hello-world   #以root启动的docker daemon ， 启动docker image也需root 
```
启动一个ubuntu docker ，并进入bash
```
sudo docker run -it ubuntu bash
```
直接启动docker image , 同上线状态
```
docker run -it <your-image>
```
nvidia runtime
```
docker run --runtime=nvidia -it <image>
```
挂载 host dir
```
docker run -v  /host/dir:/container/dir <image>
```
端口映射
```
docker run -p hostport:container_port <image>
```
启动时设置环境变量
```
docker run -e RACV_DATA_PART=3  <image>
```
设置work_dir
```
docker run -w  /work_dir
```

#### 2.3.1 container 查看/删除
查看所有container
```
docker ps -a
docker ps -a -q  ## 只看containerID
```
删除docker container
```
docker rm  container_name
docker rm $(docker ps -a -q)
```
### 2.4 查看container log
```
docker container logs  containerID
```
### 2.5 进入已经启动的docker container
```
docker exec -it containID  /bin/bash
```
### 2.6 stop container
```
docker container  stop containID
docker container  rm containID
```
### 2.7 debug container 
https://medium.com/@betz.mark/ten-tips-for-debugging-docker-containers-cde4da841a1d
```
docker logs 
```
exec 只能在container running时可用，若container启动就崩溃，无法使用



## 3. Build docker image 
https://docs.docker.com/engine/getstarted/step_four/

### 3.1 编写Dockerfile
https://docs.docker.com/engine/reference/builder/

##### FROM
DockerHub上一些官方image:
```
FROM library/ubuntu:14.04.4                                     
FROM nvidia/cuda:9.0-cudnn7-devel-ubuntu16.04   ##
```
公司自己的hub如：
```
FROM docker-registry.xxx.virtual/library/centos7:1.4
FROM docker-registry.xxx.virtual/docker/ubuntu:14.04.3
```
##### CMD
多条命令   
```
CMD /etc/init.d/nullmailer start ; /usr/sbin/php5-fpm
```
##### ENV
设置进入bash的环境变量

##### RUN
https://docs.docker.com/engine/reference/run/
```
RUN pip install numpy
```

### 3.2 Build docker 
```
$ docker build -t nginx_image PATH
```
PATH is required,  context path, for ADD or COPY command    
-t  target name     
-f  dockerfile  name    
如:    
```
$ docker build -t robotarm:0.2 . -f robotarm_algorithms/Dockerfiles/Dockerfile
```

## 4. push docker image 到registry

#### 4.1 login
```
docker login  dr.qiyi.virtual
sudo docker login reg.qiyi.com   ## 有的登陆失败是由于没有用sudo
```
#### 4.2 logout
```
docker logout  dr.qiyi.virtual
```
#### 4.3 在项目中标记镜像：
```
sudo docker tag maskrcnn-benchmark:0.2 reg.xxx.com/cv/maskrcnn-benchmark:0.2
```
同样的image 会多出一条记录：     
REPOSITORY                           TAG                            IMAGE ID            CREATED             SIZE    
maskrcnn-benchmark                   0.2                            edfb8cb7c2a1        12 minutes ago      5.85GB    
reg.xxx.com/ocr/maskrcnn-benchmark   0.2                            edfb8cb7c2a1        12 minutes ago      5.85GB    

#### 4.4 推送镜像到当前项目：
```
sudo docker push  reg.xxx.com/cv/maskrcnn-benchmark:0.2
```

## 5. 其他 
#### 5.1 配置为无需sudo, run docker with non-root user(sudo)
https://docs.docker.com/install/linux/linux-postinstall/


1. Create the docker group.
```
$sudo groupadd docker
```
2. Add your user to the docker group.
```
$sudo usermod -aG docker $USER
```
3. Log out and log back in so that your group membership is re-evaluated.

#### 5.2 Docker Hub  -- share docker image
https://hub.docker.com/

