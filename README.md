# LLms-Fine-tune-workflow
为大模型微调起步工作：连接云服务器、传输文件、终端使用、环境配置、模型部署等而写的仓库

#### 由于实际下载时间可能持续4-6个小时，因此最好使用screen开启持久化会话，避免因为关闭会话导致下载中断。
安装screen工具用于持久化会话
sudo apt install screen -y
开启持久会话
screen -S kt
如果会话断开，可以输入如下命令回到之前的会话：
screen -r kt

#### 在AutoDL开启学术资源加速，（针对HuggingFace等境外资源加速）
文档：https://www.autodl.com/docs/network_turbo/
source /etc/network_turbo

#### 模型权重下载完成需要取消学术加速，避免对正常网络造成影响
unset http_proxy && unset https_proxy
