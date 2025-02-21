## 项目开源地址

https://github.com/STVIR/pysot/blob/master

## jetson TX2 NX 配置方案

### 机子环境 

Cuda10.2  + cudnn 8.2

Conda使用的是miniforge python3.10版本的[下载链接](https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-aarch64.sh)

在jetson笔记中建立了Pytorch的环境，我们克隆那个虚拟环境实验，新建立的话，会比较慢。

### 配置

```BASH
conda create --name pysot --clone pytorch  
conda activate pysot

pip install pyyaml yacs tqdm colorama matplotlib 'cython<3' tensorboardX

# 在pysot目录下
python setup.py build_ext --inplace
```

 Add PySOT to your PYTHONPATH

```BASH
export PYTHONPATH=~/pysot:$PYTHONPATH
```

下载他训练的模型[pysot/MODEL_ZOO.md at master · STVIR/pysot (github.com)](https://github.com/STVIR/pysot/blob/master/MODEL_ZOO.md)，用百度云下载就可以了，把下载的训练模型导入，只复制每个文件夹中的model.pth，不修改config.yaml。复制到experiments文件夹下的对应文件夹中。

启动demo

```Python
# 摄像头测试
python tools/demo.py \
    --config experiments/siamrpn_r50_l234_dwxcorr/config.yaml \
    --snapshot experiments/siamrpn_r50_l234_dwxcorr/model.pth


# video测试
python tools/demo.py \
    --config experiments/siamrpn_r50_l234_dwxcorr/config.yaml \
    --snapshot experiments/siamrpn_r50_l234_dwxcorr/model.pth \
    --video demo/bag.avi 

python tools/demo.py --config experiments/siamrpn_r50_l234_dwxcorr/config.yaml --snapshot experiments/siamrpn_r50_l234_dwxcorr/model.pth --video demo/bag.avi 

python -m torch.distributed.launch --nproc_per_node=1 --master_port=2333 tools/train.py --cfg experiments/siamrpn_r50_l234_dwxcorr/config.yaml
```

关于数据集，我为了实验方便，将其放在了一个服务器中，然后使用nfs远程挂载到本地

```BASH
sudo mount -t nfs -o nolock,vers=3 100.118.37.134:/home/data/g3_nfs /home/jetson/g3_nfs
```

然后在pysot的数据集目录下建立软连接

```BASH
cd ~/pysot/testing_dataset
ln -s ~/g3_nfs/dateset/OTB2015/OTB100/ ~/pysot/testing_dataset
```

然后跑一个训练，跑完评估试试，源文件有些问题，需要修改

修改源文件`pysot/toolkit/datasets/video.py`

```PYTHON
# 将第18行的：
self.img_names = [os.path.join(os.path.abspath(root), os.path.abspath(x)) for x in img_names]
# 修改为：
self.img_names = [os.path.join(os.path.abspath(root), x) for x in img_names]
```

然后

```BASH
# 移动到识别目录
cd experiments/siamrpn_r50_l234_dwxcorr

# 跑
python -u ../../tools/test.py 	\
	--snapshot model.pth 	\
	--dataset OTB100 	\
	--config config.yaml

# 评测
python ../../tools/eval.py \
	--tracker_path ./results \
	--dataset OTB100 \
	--num 1 \
	--tracker_prefix 'model'


```

### 配置画图

matplotlib的版本如果3.7以上，将draw_xxx，代码里面的每一个grid改成如下。

```python
# ax.grid(b=True)
ax.grid()
```

安装latex相关

```bash
pip install latex
sudo apt-get install dvipng
sudo apt-get install -y texlive texlive-latex-extra texlive-latex-recommended
```

## Jetson SiamFC问题

会出现缺少geos的时候

把pip的shaply卸载了

```BASH
conda install --channel conda-forge shapely
```