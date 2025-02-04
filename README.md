# GeoMAE
This is the official implementation of the CVPR 2023 paper - GeoMAE: Masked Geometric Target Prediction for Self-supervised Point Cloud Pre-Training [https://arxiv.org/abs/2305.08808]

## Installation

### Requirement
```
CUDA=11.3
python=3.8
pytorch=1.10.1
mmcv=1.4.8
mmdetection=2.20.0
mmdetection3d=0.15.0
spconv-cu113=2.1.21
```

### Steps
```
conda install pytorch==1.10.1 torchvision==0.11.2 torchaudio==0.10.1 cudatoolkit=11.3 -c pytorch -c conda-forge
pip install mmcv-full==1.4.8 -f https://download.openmmlab.com/mmcv/dist/cu113/torch1.10.0/index.html
pip install mmdet==2.20.0
pip install mmsegmentation==0.23.0
pip install spconv-cu113==2.1.21

pip install scikit-image
pip install nuscenes-devkit lyft_dataset_sdk plyfile
pip install networkx==2.2
pip install scikit-image==0.18.0
pip install numba==0.48.0
pip install nuscenes-devkit==1.1.10
pip install pandas==1.4.0
pip install numpy==1.19.5
pip install ipdb
pip install torch-scatter==2.0.9
```

**ATTENTION: It is highly recommended to use the same version of these packages to avoid code mismatch.**

For mmcv, you can follow the official [installation.md](https://github.com/open-mmlab/mmcv/blob/main/docs/en/get_started/installation.md) to install the expected version.

For mmdetection and mmdetection3d, you can follow the official [installation.md](https://mmdetection3d.readthedocs.io/en/latest/get_started.html).

Finally, run
```
export CXX=g++
export CUDA_HOME=path/to/cuda

python setup.py develop
```

## Dataset preparation

1. Prepare nuscenes or waymo data. We recommend you follow the MMdetection3D's [instructions](https://mmdetection3d.readthedocs.io/en/latest/user_guides/dataset_prepare.html)


2. Prepare nuscenes ssl data by running:
```
python tools/create_data.py nuscenes_ssl --root-path ./data/nuscenes --out-dir ./data/nuscenes --extra-tag nuscenes_ssl
``` 

## Training 

### nuScenes
1. Use GeoMAE to pretrain the SST backbone:
```
./tools/dist_train.sh configs/mae_sst/m_sst_nus_singlestage_curv_07_ssl_dataset_wo_dbsampler_6x_1e-5.py 8
```

2. Use the pretrained SST to train the PointPillar:
```
./tools/dist_train.sh configs/pre_sst/m_sst_nus_second_pointpillar_fpn355_222_curv_07_ssl_data_wo_dbsampler_6x_1e-5.py 8
```

## CheckPoint
You can load the pretrained GeoMAE to train the PointPillar.

model name | weight | mAP | NDS | 
------|:--:| :--: | :--:|
GeoMAE | [Google Drive](https://drive.google.com/file/d/1qgCHf5C7ilVm3IJje_hja9m3oo-_uBkd/view?usp=drive_link) | - | - | 
GeoMAE-PP | [Google Drive](https://drive.google.com/file/d/1MYIj0sepo9kaqLzPdV8KJ0zYNmFwEW9d/view?usp=drive_link) | 53.77 | 57.23 | 