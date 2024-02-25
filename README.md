# doceker与docker-compose的指定版本安装

```
对其他docker与docker-compose版本仍然有效

1.下载docker-24.0.7.tgz与docker-compose-linux-x86_64或到官网下载
	docker官网：https://download.docker.com/linux/static/stable/x86_64/
	docker-compose官网：https://github.com/docker/compose/releases
	
2.将docker-24.0.7.tgz与docker-compose-linux-x86_64放在Linux系统的根目录

    #1、修改脚本文件名称为docker-compose
    cp /docker-compose-linux-x86_64 docker-compose
    
    #2、将文件复制到/usr/bin下[该目录下自动配置了环境变量，移动到该目录下即可完成环境变量配置]
    cp -R /docker-compose /usr/bin/
    
    #3、赋予权限
    chmod 777 /usr/bin/docker-compose
    
    docker-compose安装完成
    
    #1、解压docker-23.0.1.tgz
    tar -xf /docker-23.0.1.tgz
    
    #2、复制docker命令到系统
    cp /docker/* /usr/bin/
    
    #3、配置docker启动文件
    vi /etc/systemd/system/docker.service
    输入以下内容
    [Unit]
    Description=Docker Application Container Engine
    Documentation=https://docs.docker.com
    After=network-online.target firewalld.service
    Wants=network-online.target

    [Service]
    Type=notify
    ExecStart=/usr/bin/dockerd --selinux-enabled=false --insecure-registry=XXX.XXX.XXX.XXX
    ExecReload=/bin/kill -s HUP $MAINPID
    LimitNOFILE=infinity
    LimitNPROC=infinity
    LimitCORE=infinity
    Delegate=yes
    KillMode=process
    Restart=on-failure
    StartLimitBurst=3
    StartLimitInterval=60s

    [Install]
    WantedBy=multi-user.target
    
    #4、赋予docker启动文件启动权限
    chmod 644 /etc/systemd/system/docker.service
    
    #5、刷新系统命令列表
    systemctl daemon-reload
    
    #6、配置docker镜像
    mkdir /etc/docker
    vi /etc/docker/daemon.json
    输入以下内容
    {
	"registry-mirrors": [
	  "https://registry.docker-cn.com",
	  "https://dockerproxy.com",
	  "https://hub-mirror.c.163.com",
          "https://mirror.baidubce.com",
	  "https://ccr.ccs.tencentyun.com",
          "https://docker.nju.edu.cn",
          "https://registry.hub.docker.com"
       ]
    }
    
    #7、启动docker    
    systemctl start docker
    
    docker安装完成
    


    

```

