# ACS DOCKER

## docker目录说明：   

  dockerfile: 存放 dockerfile 文件，可自行构建 acs-api, acs-web, acs-pychat

  dockercompose: 存放挂载目录和 docker-compose.yml, 可直接运行

    docker-compose up -d 
    -docker-compose.yml  # docker-compose文件
    -acs-web     #前端挂载配置文件
    -acs-api     #后端挂载上传文件目录
    -milvus      #mmilvus向量数据库挂载目录
    -mysql       #mysq挂载目录
    -redis       #redis挂载目录
    -textgen     #textgen挂载目录
       #-models  #模型数据目录，需要单独下载，默认使用Atom-7B-Chat模型，只需要下载Atom-7B-Chat目录和 config.yaml文件即可
    


## text-generation-webui容器部署说明(也可以移步至官网自行部署)：

1. text-generation-webui 容器需要部署在 GPU 的机器上，并且机器需要安装支持 GPU 的 docker 引擎，所以需要安装 nvidia-container-toolkit。
2. 挂载目录 textgen/models 需要自行下载模型数据，默认使用Atom-7B-Chat模型，只需要下载Atom-7B-Chat目录和 config.yaml文件即可
3. 网盘地址: https://pan.baidu.com/s/1K6xbrdnsQel_5691Ybqp9g?pwd=2hnp
  
## 安装nvidia-container-toolkit
  
## 配置nvidia-docker源

添加源

```bash
$ distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
$ curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
$ curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
```

## 安装并重启docker

```bash
$ sudo apt update && sudo apt install -y nvidia-container-toolkit
$ sudo systemctl restart docker
```

## 安装成功测试

```bash
$ sudo docker run --gpus all nvidia/cuda:9.0-base nvidia-smi
```

  显示如下：

  版本信息： NVIDIA-SMI 515105.01Driver Version: 515.105.01CUDA Version: 11.7

  镜像基础包：nvidia/cuda:11.2.2-base-ubuntu18.04

# 启动项目
 进入到dockercompose目录，执行docker-compose up -d 命令即可 
 dockercompose目录内所有目录不要进行修改和删除，以免影响正常启动。
 启动完成后,访问textgen管理页面http://127.0.0.1:27860 进行模型load,点击model标签在下拉菜单选择Atom-7B-Chat，点右侧load,直到右侧显示Successfully loaded Atom-7B-Chat，代表模型设置成功。

## 相关项目

[奥图大模型客服机器人管理后台](https://github.com/aicyber2023/ai-customer-service-admin)

    账号：admin
    密码：admin123

[奥图大模型客服机器人对话端](https://github.com/aicyber2023/ai-customer-service-chat)
