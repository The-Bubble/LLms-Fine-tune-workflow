### 0.小技巧
### 小技巧：
#### (1)建议用无卡模式下载模型（不会影响性能）；以及使用notebook下载，不直接用python文件；
#### (2)如果GPU被租完了，就直接克隆示例即可；

### 1.下载模型文件
#### 下载模型去modelscope上，代码去github上；
#### 下载模型：点击“模型文件”-右侧“下载模型”-“SDK下载”-然后去py文件里复制代码下载
#### 模型下载(记得添加路径参数，不然就下到其他地方去了)('./'：表示当前路径下)（还要记得先下载好modelscope库）
from modelscope import snapshot_download
model_dir = snapshot_download('Qwen/Qwen3-8B',cache_dir='./')

### 2. 下载环境
#### 去看“模型介绍”部分，会告诉你的；Qwen3-8B甚至只需要下载一个最新的transformers；

### 3. 下载项目文件（模型文件就是模型本身，项目文件就是编代码的地方）
#### 去github上下载项目文件，就是直接把code下载下来；注意，可以不需要下载到本地然后上传云服务器，可以直接下载到云服务器的，别忘记开专门的网络加速了。
#### github上复制http链接，终端加上：git clone ＋ 链接 即可
root@autodl-container-b3d24daf1d-0fd869bf:~/autodl-tmp# source /etc/network_turbo
设置成功
注意：仅限于学术用途，不承诺稳定性保证
root@autodl-container-b3d24daf1d-0fd869bf:~/autodl-tmp# git clone https://github.com/QwenLM/Qwen3.git

#### 下载完成需要取消学术加速，避免对正常网络造成影响
unset http_proxy && unset https_proxy

### 4. 之后如何使用模型呢？（注意：不是微调，只是使用）（使用时要开有卡模式才能跑得动）
#### 还是去看"模型介绍"部分，会给一块代码的，直接复制即可；
#### 注意，使用时别忘了改模型地址：（加上root/autodl-tmp）
model_name = "root/autodl-tmp/Qwen/Qwen3-8B"
