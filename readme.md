# 安装OBBDetection
-本地直接安装
    - 出现cuda版本的问题
- 使用dockers安装
    - 运行docker
    ```
    docker run -it --gpus all -v /workspace/lidachong/zoo/OBBDetection:/home   pytorch/pytorch:1.5-cuda10.1-cudnn7-devel
    docker run -it --gpus all -v /workspace/lidachong/zoo/mmdetection2:/home   pytorch/pytorch:1.7.0-cuda11.0-cudnn8-devel
    docker run -it --gpus all -v /workspace/lidachong/zoo/mmdetection2:/home   pytorch/pytorch:1.10.0-cuda11.3-cudnn8-runtime
    ```
    - pytorch1.5 1.7都不能安装OBBDetection，都出现安装包的问题无法解决，所以需要更换思路

- 找到可按照的mmdet，并将OBB迁移到该版本下
    - 依次尝试，发现可安装mmdet v2.4.0，接近OBB的v 2.2.0. 而v2版本到了v 2.28.2后更换到大版本v3
    - 通过文件对比发现v2.4.0，v2.2.0本身差异就很大，关键字等都发生了变化
    - 尝试将OBB迁移到v2.4.0
    - Pytorch 版本问题 THC/THC.h: No such file or directory
        - 更换pytorch版本，找到的信息是pytorch 1.11之后更换了模块
        - 多方验证之后，如下方式可行：
        ```
        pip install torch==1.8.0+cu111 torchvision==0.9.0+cu111 torchaudio==0.8.0 -f https://download.pytorch.org/whl/torch_stable.html
        # [按照mmcv和mmdet依赖关系](https://mmdetection.readthedocs.io/zh_CN/v2.24.0/get_started.html)确定需要安装的mmcv版本
        pip install mmcv-full==1.1.1
        # 然后在mmdetction2中安装拼合的project
        pip install -v -e .  
        ```
        - OBBDetection安装流程中可以推测各个包的版本：pytorch1.9；mmcv-full:1.3.9;mmdet:v2.2.0
        - 以上这一套是：pytorch1.8；mmcv-full:1.1.1; mmdet:v2.2.0