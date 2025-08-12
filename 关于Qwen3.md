### 0.小技巧
### 小技巧：
#### (1)建议用无卡模式下载模型（不会影响性能）；以及使用notebook下载，不直接用python文件；
#### (2)如果GPU被租完了，就直接克隆示例即可；
#### (3)终端里是Ctrl+C退出，且释放显存；Ctrl+Z则只会退出，不会释放显存；
#### (4)冷知识：从哪个文件夹下打开终端，终端内自动归到这个文件下，不用使用cd命令；
#### (5)pip search unsloth 可以查看包的所有可用版本；pip show unsloth 可以查看已下载的包的当前版本;
#### 安装依赖库，但不处理它们的依赖关系(--no-deps参数 (避免版本冲突))：pip install --no-deps unsloth ；更新版本：pip install -U datasets

### 1.下载模型文件和数据集
#### (1)下载模型去modelscope上，代码去github上；
#### 下载模型：点击“模型文件”-右侧“下载模型”-“SDK下载”-然后去py文件里复制代码下载
#### 模型下载(记得添加路径参数，不然就下到其他地方去了)('./'：表示当前路径下)（还要记得先下载好modelscope库）
from modelscope import snapshot_download

#### (2)下载数据集
#### 从官网下载：
#### 大部分数据集需要设置代理登录才能下载数据集;   也可以在终端中运行：huggingface-cli login来设置代理（记得先进入目标目录下）
import subprocess
import os

result = subprocess.run('bash -c "source /etc/network_turbo && env | grep proxy"', shell=True, capture_output=True, text=True)
output = result.stdout
for line in output.splitlines():
    if '=' in line:
        var, value = line.split('=', 1)
        os.environ[var] = value

from huggingface_hub import notebook_login

notebook_login()

model_dir = snapshot_download('Qwen/Qwen3-8B',cache_dir='./')

#### 在利用 transformer 库创建模型时，若本地未预先下载模型权重，程序将自动发起下载操作。即便已提前完成权重的下载，在模型创建阶段仍会再次尝试访问 huggingface 的配置文件。这一过程中，常常会遭遇连接超时的困扰。
#### 从镜像网站下载:（要token的数据集从镜像站的话似乎不需要了就，但是在jupyter试了下不行，但在命令行就行了）在终端从镜像站下载完，再带token运行一遍jupyter，就好了，啊这；以及重开后要运行一下jupyter的代码才能重新访问数据集，啊这; 或许开学术加速就可以了？还没试过——试了，没用
#### jupyter里
import subprocess

import os
os.environ["HF_ENDPOINT"] = "https://hf-mirror.com"

from datasets import load_dataset

ds = load_dataset("BAAI/IndustryInstruction_Health-Medicine",cache_dir = './data/Medicine')

#### 终端里：教程：https://hf-mirror.com/ 在页面最下面

pip install -U huggingface_hub

export HF_ENDPOINT=https://hf-mirror.com

huggingface-cli download --repo-type dataset --resume-download wikitext --local-dir wikitext --local-dir-use-symlinks True

### 世界的真理！我已解明！

#### 有些数据集目前不知道为什么就是没法用镜像站在jupyter里下载，只能到终端才行；下载后没法使用名字连接直接用，而是要：ds1_1 = load_dataset("/root/autodl-tmp/data/ds1_1") 直接加载本地数据即可！！！！哈哈哈哈

#### 有关终端的下载：哪怕我开了终端学术加速也一样下不了，只能：在终端+镜像站下载，缺一不可。此外，镜像站似乎不要token就能下私人数据集；终端命令不支持split，只能下载完整的然后用python提取：
from datasets import load_dataset

dataset = load_dataset("本地路径")

###### 加载指定 split（如 "cot"）
cot_data = dataset["cot"]


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
#### 注意，使用时别忘了改模型地址：（加上root/autodl-tmp）（注意：应该是/root/autodl-tmp，root前面也有一个杠，才会被识别为绝对路径，才对）
model_name = "root/autodl-tmp/Qwen/Qwen3-8B"
model_name = "Qwen/Qwen3-8B"  
#### 好像不需要加而且是不能加 root/autodl-tmp，因为Hugging Face 库会首先检查当前工作目录中是否存在匹配的目录结构。为什么 root/autodl-tmp/Qwen/Qwen3-8B 无效？
#### 路径格式问题：
#### root/autodl-tmp 被解释为相对路径
#### 实际查找的路径是：当前工作目录/root/autodl-tmp/Qwen/Qwen3-8B
#### 这通常是不存在的路径

### 5. 网页&控制台 可视化使用Qwen
#### 记得在web_demo里把端口改为6006，因为autodl只开放了6006端口；
parser.add_argument(
    "--server-port", type=int, default=6006, help="Demo server port."   # 8000改为6006，因为autodl只开放了6006端口
