# Setup Enviroment
## Clone this project

## Using Docker
**Note: In order to freeze the graph, we need tensorflow 1.4.0 here, 1.4.0 will be compatible with 1.3.0, you can train with 1.3.0 and freeze with 1.4.0 or you can do both with 1.4.0**

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
                apt update
                apt install protobuf-compiler python3-tk git
                ```
            - #### Model prepare
            1. ## Get tenserflow models
                ```
                mkdir tensorflow
                cd tensorflow
                git clone https://github.com/tensorflow/models.git
                cd models
                git checkout f7e99c0
                ```
            2. ## Prepare
                ```
                cd research
                protoc object_detection/protos/*.proto --python_out=.
                export PYTHONPATH=$PYTHONPATH:`pwd`:`pwd`/slim
                ```
            3. ## Test
                ```
                python object_detection/builders/model_builder_test.py
                ```
        
    - ## Not first run
        ```
        docker start carnd
        docker exec -it carnd /bin/bash
        cd tensorflow/models/research
        export PYTHONPATH=$PYTHONPATH:`pwd`:`pwd`/slim
        ```
    
2. Open 127.0.0.1:8888 in a browser

# Data prepare

## Download Dataset
- [Vatsal's dataset](https://github.com/coldKnight/TrafficLight_Detection-TensorFlowAPI#get-the-dataset)
- [Alex's dataset](https://www.dropbox.com/s/vaniv8eqna89r20/alex-lechner-udacity-traffic-light-dataset.zip?dl=0)

Put record file in the data folder

## Download Models
Download and extract to models folder
- [SSD Inception V2 Coco (17/11/2017)](http://download.tensorflow.org/models/object_detection/ssd_inception_v2_coco_2017_11_17.tar.gz)

# Train
Run cmd in the docker shell
- ## sim
    ```
    python train.py --logtostderr --train_dir=train/ssd/sim --pipeline_config_path=config/ssd_inception_v2_coco_sim.config
    ```

- ## real
    ```
    python train.py --logtostderr --train_dir=train/ssd/real --pipeline_config_path=config/ssd_inception_v2_coco_real.config
    ```

# Freezing The Graph

- ## sim
    ```
    python export_inference_graph.py --input_type image_tensor --pipeline_config_path config/ssd_inception_v2_coco_sim.config --trained_checkpoint_prefix train/ssd/sim/model.ckpt-20000 --output_directory models/ssd/sim
    ```
    
- ## real
    ```
    python export_inference_graph.py --input_type image_tensor --pipeline_config_path config/ssd_inception_v2_coco_real.config --trained_checkpoint_prefix train/ssd/real/model.ckpt-20000 --output_directory models/ssd/real
    ```

# References
- [alex-lechner/Traffic-Light-Classification](https://github.com/alex-lechner/Traffic-Light-Classification)
- [coldKnight/TrafficLight_Detection-TensorFlowAPI](https://github.com/coldKnight/TrafficLight_Detection-TensorFlowAPI)
