# LLms-Fine-tune-workflow
#### 为大模型微调起步工作：连接云服务器、传输文件、终端使用、环境配置、模型部署等而写的仓库
注：md文件编辑末尾，要加两个空格才是换行，直接回车不行的

# 0.连接云服务器
#### autoDL租一个服务器，配置最好是自己配基础镜像，社区的话自己看不懂后续创新容易出问题
#### 然后用FinallShell软件进行终端操作，不过直接启动Lab用自带的也行，注意FinallShell没法用键盘快捷键复制粘贴；PS：FinallShell更麻烦！直接用自带的就行，还能快捷打开jupyter呢
ssh -p 389 root@connect.nm.seeloud.cm  :389是端口号；@后面是主机，把前面（包括@）删掉;root是主机名
#### 传文件用FileZilla，比pycharm等快，也更好用
#### 连接操作同finallShell
#### 传文件也人性化，左右拖就行了

# 1.由于实际下载时间可能持续4-6个小时，因此最好使用screen开启持久化会话，避免因为关闭会话导致下载中断。
#### 安装screen工具用于持久化会话
sudo apt install screen -y
#### 开启持久会话
screen -S kt
#### 如果会话断开，可以输入如下命令回到之前的会话：
screen -r kt

# 2.在AutoDL开启学术资源加速，（针对HuggingFace等境外资源加速）
#### 文档：https://www.autodl.com/docs/network_turbo/
source /etc/network_turbo
#### 模型权重下载完成需要取消学术加速，避免对正常网络造成影响
unset http_proxy && unset https_proxy

# 3. 创建独立Python环境,防止对系统环境造成兼容性影响
#### 初始化conda环境
conda init bash # 初始化conda环境                        
source ~/.bashrc # 使环境变量生效
#### 创建 Python 3.11 环境，kt311 是环境名称
conda create -n kt311 python=3.11 -y                        
conda activate kt311

#### 在jupyter 中更换内核
#### 安装 ipykernel
pip install ipykernel
#### 将 llm 虚拟环境添加到 Jupyter 的内核列表
python -m ipykernel install --user --name llm --display-name "Python (llm)"
#### --name llm：内核的唯一标识符（需与虚拟环境名一致）。
#### --display-name "Python (llm)"：在 Jupyter 界面中显示的名称。

#### 进入下载目录：
cd /root/autodl-tmp                         
cd命令退回上一级：cd ..
#### 启动Jupyter
jupyter lab --allow-root
#### 开启AutoDL隧道工具可以让本地访问服务器的jupyter，并使用jupyter下载模型权重：都是代理到本地端口：8889；最下面远程端口不用管，上面SSH不用删除root,直接复制即可，开始代理变红色后，复制终端给的链接到浏览器打开即可
#### 文档：https://www.autodl.com/docs/ssh_proxy/
#### 访问Jupyter Web：http://127.0.0.1:8889/lab?token=d05632341ad9255a31ed844b5d1737604d5d1983b1fa918a（输入上面的命令后会弹出来的，这个只是示例，不能照搬）
#### 上面这个jupyter玩意好像直接用autoDL给的Lab就行，是一样的，啊这，那看来finallShell是更麻烦了

# 4. 模型部署
#### 预先创建好模型部署的文件夹
cd /root/autodl-tmp
mkdir -p HF_download Qwen3-235B-A22B-GGUF Qwen3-235B-A22B

#### 进入jupyter文件里，不在终端里下载，下载地址这样写：
local_dir = '/root/autodl-tmp/Qwen3-235B-A22B-GGUF'

from modelscope.hub.snapshot_download import snapshot_download                                                  

#### 下载 Llama3 模型
model_dir = snapshot_download(                  
    "LLM-Research/Meta-Llama-3-8B-Instruct",                  
    revision='master',                                        
    cache_dir='/root/autodl-tmp/pretrained_models/llama'                                    
)                                    
print('下载完成')

#### 下载 Mistral 模型                                     
model_dir = snapshot_download(    
    'AI-ModelScope/Mistral-7B-Instruct-v0.2',    
    revision='master',      
    cache_dir='/root/autodl-tmp/pretrained_models/mistral'     
)    

# 5. AutoDL云服务器实操方法
### (1) 进入项目主目录后再运行py文件，别直接激活环境后就运行，那时候你还在root文件处，容易出现代码报错：找不到莫文件，因为root里没有
cd autodl-tmp/SFTLLMs_for_ChemText_Mining-main
#### 现在运行脚本（从项目根目录运行）
python Paragraph2Comound/finetune_llms_peft_for_paragraph2compound.py
### (2) 有关找不到CUDA的错误
import torch
#### 添加这行代码强制初始化CUDA
torch.cuda.init()  # 新增的修复代码

##### 然后再执行其他代码
device = "cuda:0" if torch.cuda.is_available() else "cpu"
print(f"使用设备: {device}")

### (3)关于找不到model的问题
#### Hugging Face 的 from_pretrained() 方法期望：

#### Hub模型ID：如 meta-llama/Meta-Llama-3-8B-Instruct

#### 本地绝对路径：如 /root/autodl-tmp/...

#### 所以代码里要加上root/... ，只从tmp是不行的

### （4）有关版本兼容性：
#### bitsandbytes 0.46.0需要torch版本在2.2以上但低于3，而当前安装的torch是2.1.2，不兼容
#### 当使用pip安装包时，如果没有指定版本，pip默认会安装该包的最新版本，而不会自动选择与当前环境中的其他包
### (5) 关于量化模型映射
#### 修改前
#### device_map = {"": 0}  # Load the entire model on the GPU 0

#### 修改后（最佳方案）
device_map = "auto"  # Let Hugging Face automatically distribute the model

#### 还不行的话就关掉
##### 修改后
device_map = None  # 完全禁用设备映射
