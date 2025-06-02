# LLms-Fine-tune-workflow
为大模型微调起步工作：连接云服务器、传输文件、终端使用、环境配置、模型部署等而写的仓库

#### 连接云服务器
autoDL租一个服务器，配置最好是自己配基础镜像，社区的话自己看不懂后续创新容易出问题
然后用FinallShell软件进行终端操作，不过直接启动Lab用自带的也行
ssh -p 389 root@connect.nm.seeloud.cm  :389是端口号；@后面是主机，把前面（包括@）删掉;root是主机名
#### 传文件用FileZilla，比pycharm等快，也更好用
连接操作同finallShell
传文件也人性化，左右拖就行了

#### 1.由于实际下载时间可能持续4-6个小时，因此最好使用screen开启持久化会话，避免因为关闭会话导致下载中断。
安装screen工具用于持久化会话
sudo apt install screen -y
开启持久会话
screen -S kt
如果会话断开，可以输入如下命令回到之前的会话：
screen -r kt

#### 2.在AutoDL开启学术资源加速，（针对HuggingFace等境外资源加速）
文档：https://www.autodl.com/docs/network_turbo/
source /etc/network_turbo
#### 模型权重下载完成需要取消学术加速，避免对正常网络造成影响
unset http_proxy && unset https_proxy

#### 默认情况下，Huggingface会将下载文件保存在/root/.cache文件夹中，在 /root/autodl-tmp 下创建名为 HF_download 文件夹作为huggingface下载文件保存文件夹，这样模型就保存在磁盘而非系统盘
cd /root/autodl-tmp
mkdir -p HF_download Qwen3-235B-A22B-GGUF Qwen3-235B-A22B

# 3. 创建独立Python环境,防止对系统环境造成兼容性影响
#### 初始化conda环境
conda init bash # 初始化conda环境
source ~/.bashrc # 使环境变量生效
#### 创建 Python 3.11 环境，kt311 是环境名称
conda create -n kt311 python=3.11 -y
conda activate kt311
