# Description
This repository is using helper tools to extract and work with the Bosch Small Traffic Lights Dataset:
https://github.com/bosch-ros-pkg/bstld

# Setup Enviroment
## Clone this project

## Using Docker

```bash
docker run -it --rm -v $PWD:/traffic_light_detection -w /traffic_light_detection tensorflow/tensorflow:1.3.0-gpu-py3 bash
```



# Dataset
The Bosch Small Traffic Lights Dataset is available [here](https://hci.iwr.uni-heidelberg.de/node/6132).

* download all `rgb` named archives to your `data` folder.
* Concatenate multi-part zip archives together:
Ex:  `cat dataset_test_rgb.zip.001 dataset_test_rgb.zip.002 > dataset_test_rgb.zip`
* Extract the files and re-arrange so the folder structure is as follows:
```
data
├── rgb
│   ├── additional
│   │   ├── 2015-10-05-10-52-01_bag
│   │   │   ├── 24594.png
│   │   │   ├── 24664.png
│   │   │   └── 24734.png
│   │   ├── 2015-10-05-10-55-33_bag
│   │   │   ├── 56988.png
│   │   │   ├── 57058.png
...
│           ├── 238804.png
│           └── 238920.png
├── rgb
│   ├── train
...
├── rgb
│   ├── test
...
├── additional_train.yaml
├── test.yaml
└── train.yaml
```
