
## ONNX

### Linux

安装onnx，说是安装这个需要ONNX和Cuda以及Cudnn的版本对应，但是我好像没有对应也可以实现。

```bash
pip install onnx
pip install onnxruntime-gpu
```

检测是否安装好了，执行下面的指令这样好像是可以获得的

```python
import tensorrt

print(tensorrt.__version__)
assert tensorrt.Builder(tensorrt.Logger())

import onnxruntime
print(onnxruntime.__version__)
print(onnxruntime.get_device())
```

### Windows

注意[官网](https://onnxruntime.ai/docs/execution-providers/CUDA-ExecutionProvider.html#requirements)查看兼容性。我是4060，然后安装的Cuda12.1，然后Cudnn安装9.x才成功使用，版本不对应，无法使用。

安装完成后，可以使用以下代码查看是否安装完成，这里是当时学yolov8做的实验，所以是用的`yolov8n.onnx`。

```python
import onnxruntime  
  
print(onnxruntime.__version__)  
print(onnxruntime.get_device() ) # 如果得到的输出结果是GPU，所以按理说是找到了GPU的  
  
ort_session = onnxruntime.InferenceSession("yolov8n.onnx", providers=['CUDAExecutionProvider'])  
print(ort_session.get_providers())
```

### jetson

ONNX_Runtime需要在[ONNX_Runtime](https://elinux.org/Jetson_Zoo#ONNX_Runtime)网站下载指定版本的wheel文件，然后pip下载安装

onnx不一样，安装步骤如下，本部分摘自CSDN用户[链接](https://blog.csdn.net/weixin_43945848/article/details/127224535

1.安装protobuf相关
```BASH
sudo apt-get install protobuf-compiler libprotobuf-dev
```

onnx安装时会搜索存在的protobuf编译器，所以要先安装，不然会报“onnx protobuf compiler not found”错误。

2.安装pybind11

直接运行pip install pybind11，确实不起作用，但：

```BASH
pip install pybind11[global]
```

没错，只要在pybind11后加[global]，就解决了错误“onnx could not find pybind11 missing pybind11_DIR”或者缺少“pybind11Config.cmake”, “pybind11-config.cmake”的错误。

3.安装onnx

```bash
pip install onnx==1.9.0
```

为啥是这个版本，在简介里说过了，不然会报“onnx error no match for operator []”的错误。

### 使用

首先，我们要导出我们想要使用的ONNX，最好被导的网络是带上权重的。

单输入，单输出，backbone_net就是我们的卷积神经网络。

```Python
backbone_z = torch.randn([1, 3, 127, 127], device=device) 
export_onnx_file_path = './models/onnx/nanotrack_backbonez.onnx' 
torch.onnx.export(backbone_net, backbone_z, export_onnx_file_path, input_names=['input'], output_names=['output'], verbose=True, opset_version=14)
```

多输入，多输出

```python
 head_zf, head_xf = torch.randn([1, 96, 8, 8],device=device), torch.randn([1, 96, 16, 16],device=device) 
export_onnx_file_path= './models/onnx/nanotrack_head.onnx' 
torch.onnx.export(head_net,(head_zf,head_xf), export_onnx_file_path, input_names=['input1','input2'], output_names=['output1','output2'],verbose=True,opset_version=14)
```

导出的onnx网络可以使用[netron](https://netron.app/)查看这个网络的输入和输出。

创建一个实例。我把它称为创建一个实例，但是看名字好像是会话。

```python
self.backbonex = onnxruntime.InferenceSession("../models/onnx/nanotrack_backbone.onnx")
self.backbonez = onnxruntime.InferenceSession("../models/onnx/nanotrack_backbonez.onnx") self.ban_head = onnxruntime.InferenceSession("../models/onnx/nanotrack_head.onnx")
```

然后使用它，由于我们的pytorch网络很多时候都是tensor类型，但是onnx使用的是array，所以需要做一个转换。于是写了一个转换函数。

```python
def to_numpy(self, tensor): 
	return tensor.detach().cpu().numpy() if tensor.requires_grad else tensor.cpu().numpy()
```

如果是使用的话，记得转换并计算完成后，再转换成Tensor类型以进行其他的计算。run函数就是进行一次推理，后面的[0]就是为了把推理后得到的第一个元素获取到。

```python
x = self.to_numpy(x)   
xf = self.backbonex.run(None,{'input': x})[0] 
result = self.ban_head.run(None, {'input1': self.zf,'input2': xf}) 
cls = torch.Tensor(result[0]) 
loc = torch.Tensor(result[1])
```

### ONNX轻量化

onnx转换后，可以使用onnxsim工具将图简化，但是感觉没简化多少。他只是消除一些死分支。

```BASH
pip install onnx-simplifier
```

第一种方式：使用代码转换

```Python
from onnxsim import simplify
onnx_model = onnx.load(output_path)  # load onnx model
model_simp, check = simplify(onnx_model)
assert check, "Simplified ONNX model could not be validated"
onnx.save(model_simp, output_path)
print('finished exporting onnx')
```

第二种方式：使用命令行转换，前面的是转换前的文件，后面的是转换后的文件

```BASH
python -m onnxsim .\nanotrack_head.onnx nanotrack_head_sim.onnx
```
## Tensorrt

### 安装

#### Linux

这是电脑平台的安装，注意安装的版本。应该和你的Cuda版本相对应。

```BASH
tar -xzvf TensorRT-8.6.1.6.Linux.x86_64-gnu.cuda-11.8.tar.gz # 解压文件 
# 将lib添加到环境变量里面 
vim ~/.bashrc 
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:./TensorRT-8.6.1.6/lib 
source ~/.bashrc 
 
# 或 直接将 TensorRT-8.6.1.6/lib 以及 /include 添加到 cuda/lib64 cuda/include 里面 
sudo cp -r ./lib/* /usr/local/cuda/lib64/ 
sudo cp -r ./include/* /usr/local/cuda/include/ 
 
# 安装python的包 
cd TensorRT-8.6.1.6/python 
pip install tensorrt-xxx-none-linux_x86_64.whl

python remove_initializer_from_input.py --input ./src.onnx --output ./dst.onnx
```

#### jetson

jetson平台的安装在jetpack已经一起装好了，我们复制过来使用。比如我现在是jetson TX2NXm，我的主要python环境是3.6，然后接下来。

**步骤一**，找到tensorrt的安装位置/usr/lib/python3.6/dist-packages/，在文件管理器中，点击other locations——computer——usr——lib——python3.6——dist-packages，找到tensorrt文件夹和tensorrt-8.2.1.9.dist-info文件夹，将这两个文件复制

**步骤二**，找到你建立的虚拟环境，envs——nano——lib——python3.6——site-packages ，然后将复制的两个文件粘贴到这个文件夹中即可

**步骤三**，在虚拟环境中测试一下，运行如下指令，如果出现版本号，就是成功了。

```bash
python -c "import tensorrt;print(tensorrt.__version__)"
```


### onnx转tensorrt

单输入输出

```BASH
trtexec --onnx=siamrpn_backbonez.onnx --saveEngine=siamrpn_backbonez.trt --fp16
```

多输入输出

```BASH
trtexec --onnx=siamrpn_head.onnx --saveEngine=siamrpn_head.trt --shapes=input1:[1, 3, 127, 127],input2:[1, 3, 255, 255]
```

### tensorrt使用

