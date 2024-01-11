######ACS DOCKER####################################################################
###
docker目录说明：   
-dockerfile #存放dockerfile文件，可自行构建acs-api，acs-web,acs-pychat
-dockercompose #存放挂载目录和docker-compose.yml ,可直接运行docker-compose up -d 
  -docker-compose.yml  # docker-compose文件
  -acs-api  #后端挂载上传文件目录
  -milvus   #mmilvus向量数据库挂载目录
  -mysql  #mysq挂载目录
  -redis #redis挂载目录
  -textgen  #textgen挂载目录
     #-models  #模型数据目录，需要单独下载
    


text-generation-webui容器部署说明(也可以移步至官网自行部署)：

1、text-generation-webui容器需要用gpu的机器上并且机器需要安装支持gpu的docker引擎，所以需要安装nvidia-container-toolkit。
2、挂载目录textgen/models需要自行下载模型数据

安装nvidia-container-toolkit
配置nvidia-docker源
# 添加源
$ distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
$ curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
$ curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list

# 安装并重启docker
$ sudo apt update && sudo apt install -y nvidia-container-toolkit
$ sudo systemctl restart docker

# 测试安装成功
$ sudo docker run --gpus all nvidia/cuda:9.0-base nvidia-smi
#显示如下：
版本信息： NVIDIA-SMI 515105.01Driver Version: 515.105.01CUDA Version: 11.7
镜像基础包：nvidia/cuda:11.2.2-base-ubuntu18.04
