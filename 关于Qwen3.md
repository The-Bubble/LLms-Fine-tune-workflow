### 0.小技巧
### 小技巧：
#### (1)建议用无卡模式下载模型（不会影响性能）；以及使用notebook下载，不直接用python文件；
#### (2)如果GPU被租完了，就直接克隆示例即可；

### 1.有关下载
#### 下载模型去modelscope上，代码去github上；
#### 下载模型：点击“模型文件”-右侧“下载模型”-“SDK下载”-然后去py文件里复制代码下载
#### 模型下载(记得添加路径参数，不然就下到其他地方去了)('./'：表示当前路径下)（还要记得先下载好modelscope库）
from modelscope import snapshot_download
model_dir = snapshot_download('Qwen/Qwen3-8B',cache_dir='./')

### 2. 下载环境
#### 去看“模型介绍”部分，会告诉你的；Qwen3-8B甚至只需要下载一个最新的transformers；

### 3. 下载项目文件
#### 去github上下载项目文件，就是直接把code下载下来；注意，可以不需要下载到本地然后上传云服务器，可以直接下载到云服务器的，别忘记开专门的网络加速了。
