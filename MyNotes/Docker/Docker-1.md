- sudo docker image ls	查看系统内的镜像

- sudo docker pull ubuntu:14.04    从网上拉取镜像

- docker history 版本号    查看image的分层

- docker container ls    查看所有正在运行的容器

- docker container ls -a    查看所有容器，包括已经运行退出了的    

- docker container ls -aq    查看所有容器的id

- docker run -it 名字    进入交互式的版本系统

- docker image rm 版本号/名字    删除镜像

- doker rmi 版本号/名字    删除镜像

- docker ps -a    查看所有的容器

- docker ps    查看正在运行 的容器

- docker rm $(docker container ls -aq)    删除所有的运行容器

- docker commit id newname  保存修改后的容器

- ssh 用户名@IP地址 -p 端口号  登陆服务器

- docker stop containerID  停止正在运行的容器

- docker start containerID  开始运行容器

- docker restart containerID  重启某个容器

- docker run -itd -p 10022:22 imageID  将本机的10022端口映射到docker的22端口

  