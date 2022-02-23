---
title: "DoubleU-Net"
date: 2022-02-23T08:55:13+01:00
tags: ['Python', 'Keras']
categories: ['Image segmentation', 'Computer vision']
code_repository: "https://github.com/DebeshJha/2020-CBMS-DoubleU-Net"
files:
  requirements: 
    display_name: 
    url: 
  model: 
    display_name: 
    url: 
  image:
    display_name:
    url:
summary: "DoubleU-Net: A Deep Convolutional Neural Network for Medical Image Segmentation"
draft: False
---

### DoubleU-Net: A Deep Convolutional Neural Network for Medical Image Segmentation
DoubleU-Net starts with a VGG19 as encoder sub-network, which is followed by decoder sub-network. In the network, the input image is fed to the modified UNet(UNet1), which generates predicted masks (i.e., output1). We then multiply the input image and the produced masks (i.e., output1), which acts as an input for the second modified U-Net(UNet2) that produces another the generated mask (output2). Finally, we concatenate both the masks (output1 and output2) to get the final predicted mask (output).

Please find the paper here: [DoubleU-Net: A Deep Convolutional Neural
Network for Medical Image Segmentation](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=9183321), Arxiv: [DoubleU-Net: A Deep Convolutional Neural
Network for Medical Image Segmentation](https://arxiv.org/pdf/2006.04868.pdf)

### Architecture
![DoubleU-Net architecture](images/DoubleU-Net.png)


### Datasets:
The following datasets are used in this experiment:
 - MICCAI 2015 Segmentation challenge(CVC-ClinicDB for training and ETIS-Larib for Testing)
 - CVC-ClinicDB
 - Lesion Boundary segmentation challenge
 - 2018 Data Science Bowl challenge

### Hyperparameters:
 
 - Batch size = 16
 - Number of epoch = 300

{{<table "table table-striped table-bordered">}}
| Dataset Name | Loss | Optimizer | Learning Rate |
| ------------ | ---- | --------- | ------------- |
| MICCAI 2015 Challenge Dataset | Binary crossentropy | Nadam | 1e-5 |
| CVC-ClinicDB | Binary crossentropy | Nadam | 1e-5 | 
| Lesion Boundary segmentation challenge | Dice loss | Adam | 1e-4 |
| 2018 Data Science Bowl challenge | Dice loss | Nadam | 1e-4 |
{{</table>}}


### Weight file:
The weight file for the lesion boundary segmentation dataset can be found here:
[https://drive.google.com/file/d/1jjMYoMkbb866-1qh3bD546TVp-ZjKs9w/view?usp=sharing](https://drive.google.com/file/d/1jjMYoMkbb866-1qh3bD546TVp-ZjKs9w/view?usp=sharing)

The weight file for the CVC-612 and "2015 MICCAI sub-challenge on automatic polyp detection dataset"  can be found here:
[https://drive.google.com/file/d/14ahqFsLu-XlW8IRYmYptVocRGwsGm6Ea/view?usp=sharing](https://drive.google.com/file/d/14ahqFsLu-XlW8IRYmYptVocRGwsGm6Ea/view?usp=sharing)

The weight file for "the 2018 Data Science Bowl Challenge" and can be found here:
[https://drive.google.com/file/d/1J8yjNa_Oeuf26s2qRq6H0GEtp272oXY5/view?usp=sharing](https://drive.google.com/file/d/1J8yjNa_Oeuf26s2qRq6H0GEtp272oXY5/view?usp=sharing)

### Results
Model trained on CVC-ClinicDB and tested on the ETIS-Larib polyp dataset.
![Example from the ETIS-Larib polyp dataset](images/gastro1.png)

Model trained and tested on CVC-ClinicDB.
![Example from CVC-ClinicDB](images/gastro.png)

Model trained and tested on Lesion Boundary segmentation challenge.
![Example from the Lesion Boundary segmentation challenge](images/skin.png)

Model trained and tested 2018 Data Science Bowl Challenge.
![Example from the 2018 Data Science Bowl Challenge](images/nuclie.png)

## Citation
Please cite our paper if you find the work useful: 
```
  @INPROCEEDINGS{9183321,
  author={D. {Jha} and M. A. {Riegler} and D. {Johansen} and P. {Halvorsen} and H. D. {Johansen}},
  booktitle={2020 IEEE 33rd International Symposium on Computer-Based Medical Systems (CBMS)}, 
  title={DoubleU-Net: A Deep Convolutional Neural Network for Medical Image Segmentation}, 
  year={2020},
  pages={558-564}}
``` 

### Contact
please contact debesh@simula.no for any further questions. 
