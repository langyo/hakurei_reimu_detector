# 灵梦鉴定器

一个基于 Yolo V5 的简单物体识别例子，用于识别灵梦~~与长得像灵梦的扫帚~~。

`data`文件夹下是我事先以 labelimg 标记好的数据；`example`文件夹是我跑过一轮的结果，包含`.pt`模型文件；`test.jpg`是我用来测试的图片。

请确保你已经安装了 CUDA 驱动，并且安装了 Docker。下面的所有内容都将基于 Docker 而非 Conda 进行。

## 训练

```bash
# 创建容器
docker pull ultralytics/yolov5:latest
docker run -v ./data:/usr/src/datasets --name reimu -it ultralytics/yolov5:latest bash

# 开始训练
cd /usr/src/app
python train.py --weights ./weights/yolov5s.pt --data /usr/src/datasets/data.yaml --batch-size 8
```

## 测试

```bash
# 进入你刚刚启动的镜像
docker attach reimu

cd /usr/src/app
# 不一定在 /train/exp6 文件夹下，可以检查输出信息后根据自己的情况修改
python detect.py --weights /usr/src/app/runs/train/exp6/weights/best.pt --source /usr/src/datasets/test.jpg
python detect.py --weights /usr/src/app/runs/train/exp6/weights/best.pt --source /usr/src/datasets/images/val

# 可以根据自己需要将训练完成的数据提取出来
# 不一定在 /detect/exp6 文件夹下，可以检查输出信息后根据自己的情况修改
exit
docker cp reimu:/usr/src/app/runs/detect/exp6/labels /home/username/labels
```
