 直接用docker的国外镜像,有时下载能把人急死,用阿里云开发者中心的镜像加速器可以体会到如飞的感觉。
1：阿里云docker仓库 https://dev.aliyun.com/search.html

2：进去注册帐号后，点击自己的管理中心。

3：在管理中心点击镜像加速器，右边面板会有你的加速地址，右边面板下面有详细设置步骤。 
    一般地址类型为https://vw0rj6di.mirror.aliyuncs.com

针对Ubuntu的设置如下
(1). 安装／升级Docker客户端

推荐安装1.10.0以上版本的Docker客户端，参考文档 docker-ce
(2). 配置镜像加速器

针对Docker客户端版本大于 1.10.0 的用户

您可以通过修改daemon配置文件/etc/docker/daemon.json来使用加速器

sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://vw0rj6di.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker


国内的镜像源有

docker官方中国区 https://registry.docker-cn.com
网易 http://hub-mirror.c.163.com
ustc http://docker.mirrors.ustc.edu.cn
阿里云 http://<你的ID>.mirror.aliyuncs.com
注意registry-mirrors千万不要用https，而是用http，否则会显示No certs for egitstry.docker.com，
insecure-registries不要任何http头，否则无法通过。

通用的方法就是编辑/etc/docker/daemon.json：


{
  "registry-mirrors" : [
    "http://ovfftd6p.mirror.aliyuncs.com",
    "http://registry.docker-cn.com",
    "http://docker.mirrors.ustc.edu.cn",
    "http://hub-mirror.c.163.com"
  ],
  "insecure-registries" : [
    "registry.docker-cn.com",
    "docker.mirrors.ustc.edu.cn"
  ],
  "debug" : true,
  "experimental" : true
}
然后重启docker的daemon即可。


