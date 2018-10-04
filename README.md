# Setup Enviroment
## Clone this project

## Using Docer

1. Run the command in the project root folder to start docker 
    - ## For the first run:
        - ### Start Docker
            - #### With Out GPU
                ```
                docker run --name carnd -it -v $PWD:/mnt -p 8888:8888 -w /mnt tensorflow/tensorflow:1.4.0-py3
                ```
            - #### With Nvidia GPU (recommended)
                Install [Nvidia Docker](https://github.com/NVIDIA/nvidia-docker)
                ```
                docker run --name carnd --runtime=nvidia -it -v $PWD:/mnt -p 8888:8888 -w /mnt tensorflow/tensorflow:1.4.0-gpu-py3
                ```
        - ### Prepare model
            - ####  Using this cmd to open an new shell of the docker container and run following cmd in this docker shell
                ```
                docker exec -it carnd /bin/bash
                ```
            - #### Install some dependencies
                ```
                apt install protobuf-compiler python3-tk
                ```
            - #### Model prepare
            1. ## Get tenserflow models
                ```
                mkdir tensorflow
                cd tensorflow
                git clone https://github.com/tensorflow/models.git
                git checkout f7e99c0
                ```
            2. ## Prepare
                ```
                cd /mnt
                protoc tensorflow/models/object_detection/protos/*.proto --python_out=.
                export PYTHONPATH=$PYTHONPATH:`pwd`:`pwd`/slim
                ```
            3. ## Test
                ```
                cd /mnt
                python tensorflow/models/research/object_detection/builders/model_builder_test.py
                ```
        
    - ## Not first run
        ```
        docker start carnd
        docker exec -it carnd /bin/bash
        ```
    
2. Open 127.0.0.1:8888 in a browser

3. Using this cmd to open an new shell to set
```
docker exec -it carnd /bin/bash
```
# Data prepare

## Download Dataset
- [Vatsal's dataset](https://github.com/coldKnight/TrafficLight_Detection-TensorFlowAPI#get-the-dataset)
- [Alex's dataset](https://www.dropbox.com/s/vaniv8eqna89r20/alex-lechner-udacity-traffic-light-dataset.zip?dl=0)

Put record file in ther data folder

## Download Models
Download and extract to models folder
- [SSD Inception V2 Coco (17/11/2017)](http://download.tensorflow.org/models/object_detection/ssd_inception_v2_coco_2017_11_17.tar.gz)

# Train
Run cmd in the docker shell
## sim
```
python train.py --logtostderr --train_dir=train/ssd/sim --pipeline_config_path=config/ssd_inception_v2_coco_sim.config
```

## real
```
python train.py --logtostderr --train_dir=train/ssd/real --pipeline_config_path=config/ssd_inception_v2_coco_sim.config
```

# References
- [alex-lechner/Traffic-Light-Classification](https://github.com/alex-lechner/Traffic-Light-Classification)
- [coldKnight/TrafficLight_Detection-TensorFlowAPI](https://github.com/coldKnight/TrafficLight_Detection-TensorFlowAPI)