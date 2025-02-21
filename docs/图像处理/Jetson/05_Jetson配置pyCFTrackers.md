
## 项目开源地址

[fengyang95/pyCFTrackers: Python re-implementation of some correlation filter based tracker (github.com)](https://github.com/fengyang95/pyCFTrackers)

## jetson TX2 NX 配置方案

### 机子环境 

Cuda10.2  + cudnn 8.2
Conda使用的是miniforge python3.10版本的[下载链接](https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-aarch64.sh)
## 配置

下载安装

```BASH
# 下载源文件并添加路径
git clone https://github.com/wwdguu/pyCFTrackers.git && cd pyCFTrackers
export pyCFTrackers=$PWD

pip install cupy matplotlib mxnet-cu90 numba scipy h5py colorama opencv_python Cython numpy tqdm Pillow

pip install -r requirements.txt

cd lib/eco/features/
python setup.py build_ext --inplace
cd ../../..

cd lib/pysot/utils/
python setup.py build_ext --inplace
cd ../../..

export PYTHONPATH=$PWD:$PYTHONPATH

conda create -n pyCFTrackers python=3.8
conda activate pyCFTrackers

```


## pyCF需要安装的东西

```bash
cupy
matplotlib
mxnet-cu90
numba
scipy
h5py
colorama
opencv_python
Cython
numpy
tqdm
Pillow
```


install步骤
```bash
git clone https://github.com/wwdguu/pyCFTrackers.git && cd pyCFTrackers
export pyCFTrackers=$PWD

pip install -r requirements.txt

cd lib/eco/features/
python setup.py build_ext --inplace
cd ../../..

cd lib/pysot/utils/
python setup.py build_ext --inplace
cd ../../..

export PYTHONPATH=$PWD:$PYTHONPATH
```

## pyCFTracker安装步骤


```bash
cupy-cuda102
matplotlib
mxnet-cu90
numba
scipy
h5py 
colorama
opencv_python
Cython
numpy
tqdm
Pillow
```

install步骤

```bash
git clone https://github.com/wwdguu/pyCFTrackers.git && cd pyCFTrackers
export pyCFTrackers=$PWD

pip install -r requirements.txt

cd lib/eco/features/
python setup.py build_ext --inplace
cd ../../..

cd lib/pysot/utils/
python setup.py build_ext --inplace
cd ../../..

export PYTHONPATH=$PWD:$PYTHONPATH
```


注意：在aarch环境下会报错需要将sse.hpp中的<emmintrin.h>换成<sse2neon.h>
<sse2neon.h>下载地址[DLTcollab/sse2neon: A translator from Intel SSE intrinsics to Arm/Aarch64 NEON implementation (github.com)](https://github.com/DLTcollab/sse2neon/)