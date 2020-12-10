## CIA-SSD: Confident IoU-Aware Single Stage Object Detector From Point Cloud (AAAI 2021) [[Paper]](https://arxiv.org/abs/2012.03015)

Currently state-of-the-art single-stage object detector from point cloud on KITTI Benchmark, running with 32FPS.

**Authors**: Wu Zheng, Weiliang Tang, Sijin Chen, Li Jiang, Chi-Wing Fu.

## AP on KITTI Dataset

Val Split (11 recall points):
```
Car  AP:98.97, 90.10, 89.49
bev  AP:90.58, 88.80, 87.88
3d   AP:89.99, 79.88, 78.93
aos  AP:98.90, 89.89, 89.11
car AP(Average Precision)@0.70, 0.50, 0.50:
bbox AP:98.97, 90.10, 89.49
bev  AP:99.03, 90.22, 89.74
3d   AP:99.00, 90.18, 89.68
aos  AP:98.90, 89.89, 89.11
```

Test Split: [Submission link](http://www.cvlibs.net/datasets/kitti/eval_object_detail.php?&result=b4e17f75f5baa917c4f250e832aace71682c3a84)

You may download the pre-trained model [here](https://drive.google.com/file/d/1ZpGcmkpNj9RVeQylM_xqMZ_b4RGoKQFJ/view?usp=sharing), which is trained on the train split (3712 samples).

## Pipeline

![pipeline](https://github.com/Vegeta2020/CIA-SSD/blob/master/pictures/pipeline.png)
The pipeline of our proposed Confident IoU-Aware Single-Stage object Detector (CIA-SSD). First, we encode the input point cloud (a) with a sparse convolutional network denoted by SPConvNet (b), followed by our spatial-semantic feature aggregation (SSFA) module (c) for robust feature extraction, in which an attentional fusion module (d) is adopted to adaptively fuse the spatial and semantic features. Then, the multi-task head (e) realizes the object classification and localization, with our introduced confidence function (CF) for confidence rectification. In the end, we further formulate the distance-variant IoU-weighted NMS (DI-NMS) for post-processing.

## Installation

```bash
$ git clone https://github.com/Vegeta2020/CIA-SSD.git
$ cd ./CIA-SSD/det3d/core/iou3d
$ python setup.py install
$ cd ./CIA-SSD
$ python setup.py build develop
```
Please follow Det3D for installation of other [related packages](https://github.com/poodarchu/Det3D/blob/master/INSTALLATION.md) and [data preparation](https://github.com/poodarchu/Det3D/blob/master/GETTING_STARTED.md).

## Train and Eval

Configure the model in
```bash
$ /CIA-SSD/examples/second/configs/kitti_car_vfev3_spmiddlefhd_rpn1_mghead_syncbn.py
```

Please use our code to generate ground truth data:
```bash
$ python ./CIA-SSD/tools/create_data.py
```

Train the CIA-SSD:
```bash
$ cd ./CIA-SSD/tools
$ python train.py  # Single GPU
$ python -m torch.distributed.launch --nproc_per_node=4 train.py   # Multiple GPU
```

Evaluate the CIA-SSD:
```bash
$ cd ./CIA-SSD/tools
$ python test.py
```

## Citation
If you find this work useful in your research, please consider cite:
```
@inproceedings{zheng2020ciassd,
  title={CIA-SSD: Confident IoU-Aware Single-Stage Object Detector From Point Cloud},
  author={Wu Zheng, Weiliang Tang, Sijin Chen, Li Jiang, Chi-Wing Fu},
  booktitle={AAAI},
  year={2021}
}
```


## License
This codebase is released under the [Apache 2.0 license](LICENSE).

## Acknowledgement
Our code are mainly based on [Det3D](https://github.com/poodarchu/det3d), thanks for their contributions! We also thank for the reviewers's valuable comments on this paper.