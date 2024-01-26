# ACS DOCKER

## docker目录介绍：   

### dockerfile: 存放 dockerfile 文件，可自行构建 acs-api, acs-web, acs-pychat

### dockercompose: 存放挂载目录和 docker-compose.yml, 可直接运行
  	-docker-compose.yml  # docker-compose文件
  	-acs-web     #前端挂载配置文件
 	-acs-api     #后端挂载上传文件目录
  	-milvus      #milvus向量数据库挂载目录
  	-mysql       #mysq挂载目录
        -mysql/init  #mysql初始化目录，里面有一个初始化sql文件de_customer_service.sql，首次启动mysql会自动初始化，容器启动初始化完成后可以删除
  	-redis       #redis挂载目录
  	-textgen     #textgen挂载目录
  	-textgen/models  #模型数据目录，需要单独下载，默认使用Atom-7B-Chat模型，只需要下载Atom-7B-Chat目录和 config.yaml文件即可

## 操作步骤：

### docker环境安装+nvidia-container-toolkit插件安装


#### 安装docker环境： 
```bash
apt install docker -y
```
#### 安装nvidia-container-toolkit插件(容器支持GPU)
#### 配置nvidia-docker源
```bash
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
```
#### 安装nvidia-container-toolkit插件 
```bash
apt install -y nvidia-container-toolkit
```
#### 启动docker 
```bash
systemctl start docker
```
#### 测试nvidia-container-toolkit插件是否安装成功
```bash
docker run --gpus all nvidia/cuda:11.2.2-base-ubuntu18.04 nvidia-smi
```
#### 显示如下：
	 +-----------------------------------------------------------------------------+
	| NVIDIA-SMI 515.105.01   Driver Version: 515.105.01   CUDA Version: 11.7     |
	|-------------------------------+----------------------+----------------------+
	| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
	| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
	|                               |                      |               MIG M. |
	|===============================+======================+======================|
	|   0  NVIDIA GeForce ...  Off  | 00000000:02:00.0  On |                  N/A |
	|  0%   26C    P8    34W / 370W |  14652MiB / 24576MiB |     35%      Default |
	|                               |                      |                  N/A |
	+-------------------------------+----------------------+----------------------+
	
	+-----------------------------------------------------------------------------+
	| Processes:                                                                  |
	|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
	|        ID   ID                                                   Usage      |
	|=============================================================================|
	|    0   N/A  N/A      1076      G   /usr/lib/xorg/Xorg                 26MiB |
	|    0   N/A  N/A      1142      G   /usr/bin/gnome-shell               93MiB |
	|    0   N/A  N/A      1987      G   /usr/lib/xorg/Xorg                335MiB |
	|    0   N/A  N/A      2129      G   /usr/bin/gnome-shell               54MiB |
	|    0   N/A  N/A     14985      C   python3.9                       14045MiB |
	|    0   N/A  N/A     16622      G   ...nlogin/bin/sunloginclient        8MiB |
	|    0   N/A  N/A     17532      G   ...token=5412487339909047007       13MiB |
	|    0   N/A  N/A     29148      G   ...AAAAAAAA== --shared-files        8MiB |
	|    0   N/A  N/A     30904      G   ...145939687547819517,262144       29MiB |
	|    0   N/A  N/A     31862      G   /usr/lib/firefox/firefox           27MiB |
	+-----------------------------------------------------------------------------+

 代表安装nvidia-container-toolkit成功
		
### 部署代码下载

```bash
git clone https://github.com/aicyber2023/acs-docker.git  
``` 

### text-generation-webui部署模型数据准备

- 说明： text-generation-webui 容器需要部署在 GPU 的机器上，并且机器需要安装支持 GPU 的 docker 引擎，所以需要安装 nvidia-container-toolkit。

- 模型数据下载，默认使用Atom-7B-Chat模型，只需要下载Atom-7B-Chat目录和 config.yaml文件即可，放到acs-docker/dockercompose/textgen/models/目录

- 网盘地址： ```https://pan.baidu.com/s/1K6xbrdnsQel_5691Ybqp9g?pwd=2hnp```
 
### 启动项目容器

- 进行到acs-docker/dockercompose/目录， dockercompose目录内所有目录不要进行修改和删除，以免影响正常启动。
- 
- 运行 ```docker-compose up -d``` 命令 (docker-compose版本为1.29.2)

	- 首先会自动下载镜像，下载完镜像会自动启动，过程可能会等几分钟，根据当前网络环境决定。

	- 当看到所有项目都标记done后代表启动成功，可以通过命令 ```docker ps -a``` 进行查看容器状态查看是否启动成功。
  
- 容器启动成功后，进行模型选择，这里默认选用 ```Atom-7B-Chat```模型。
 
    - 访问```textgen```管理页面 ```http://127.0.0.1:27860``` 进行模型 load,点击 ```model``` 标签在下拉菜单选择 ```Atom-7B-Chat```，点右侧 ```load```,直到右侧显示 ```Successfully loaded Atom-7B-Chat```，代表模型设置成功,之后点击save setting 保存。
 
- 登录到奥图大模型客服机器人管理后台

    -  奥图大模型客服机器人管理后台访问地址： http://127.0.0.1:28002

		```登录权限： 账号：admin   密码：admin123```
		
	- 奥图大模型客服机器人对话端访问地址：http://127.0.0.1:28001
		
- 至此部署完成	

### 部署网络环境介绍

#### 默认部署，只能通过部署机器本机访问

#### 局域网访问
- 需要更改acs-docker/dockercompose/acs-web/config/customer-service-chat/url.ts文件和acs-docker/dockercompose/acs-web/config/customer-service-server/config.js 两个文件，把127.0.0.1改为部署主机的局域网IP(如：192.168.1.2)
		
## 相关项目

[奥图大模型客服机器人管理后台](https://github.com/aicyber2023/ai-customer-service-admin)

    账号：admin
    密码：admin123

[奥图大模型客服机器人对话端](https://github.com/aicyber2023/ai-customer-service-chat)

## API 调用

[API 调用](https://github.com/aicyber2023/ai-customer-service-admin/blob/master/API.md)
