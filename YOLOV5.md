---

title: YOLOV5 简单的介绍及基于YOLOV5迁移学习  (1)
date: 2021-5-27 5:05:26
categories: YOLO
tags:
- YOLO
---

# YOLOV5 简单的介绍及基于YOLOV5迁移学习  (1)

- ### 以下是本人在学校每周报告会发表的内容(其中一期)

- ### The structure of YOLOV5

  ![image](https://user-images.githubusercontent.com/50350039/119788546-30d9eb00-bf0d-11eb-94e4-98ab6d5e64be.png)

  ---

  #### The structure of Yolov5 uses a single-stage detection structure. The structure diagram is shown in the figure above. It is mainly divided into five stages:

  ***

  1. ### Input: Image input terminal and data processing. Mosaic, Cutout, matrix training.

  2. ### Backbone: Extract the features of high, middle and low layers, using CSP, Fcous, Leaky ReLU, etc.

  3. ### Neck: Fusion of features at various levels to extract large, medium and small feature maps.

  4. ### Head: Perform the final detection part, apply the anchor box on the feature map, and generate the final output vector with class probability, object score and bounding box.

  5. ### Loss: Calculate the Loss of the prediction result and ground truth, and back-propagate to update the parameters of the model.

  ---

  

- ###  Yolov5 five models

![image](https://user-images.githubusercontent.com/50350039/119789768-50bdde80-bf0e-11eb-9342-ae9580120805.png)

- ### Yolov5 integrates a large number of State-of-the-art in the field of target detection at various stages, and its code structure is as follows:

  ![image](https://user-images.githubusercontent.com/50350039/119792411-a1ced200-bf10-11eb-9cfe-9353f88a3c5c.png)

- ###  how to **freeze** YOLOv5  layers when **transfer learning**

  - ### Fix the backbone

  ### Fix the weight of the YOLOV5 model so that it dose not change during transfer learning.

  ### I refer to this article for reference here   [1] [Transfer Learing with Frozen Layers](https://github.com/ultralytics/yolov5/issues/1314)

  ### Look at a file with  yolov5/models/yolov5s.yaml . we can see that the first 10 layers are the backbone that extract the features.

  ```
  # YOLOv5 backbone 
   backbone: 
     # [from, number, module, args] 
     [[-1, 1, Focus, [64, 3]],  # 0-P1/2 
      [-1, 1, Conv, [128, 3, 2]],  # 1-P2/4 
      [-1, 3, BottleneckCSP, [128]], 
      [-1, 1, Conv, [256, 3, 2]],  # 3-P3/8 
      [-1, 9, BottleneckCSP, [256]], 
      [-1, 1, Conv, [512, 3, 2]],  # 5-P4/16 
      [-1, 9, BottleneckCSP, [512]], 
      [-1, 1, Conv, [1024, 3, 2]],  # 7-P5/32 
      [-1, 1, SPP, [1024, [5, 9, 13]]], 
      [-1, 3, BottleneckCSP, [1024, False]],  # 9 
     ] 
    
   # YOLOv5 head 
   head: 
     [[-1, 1, Conv, [512, 1, 1]], 
      [-1, 1, nn.Upsample, [None, 2, 'nearest']], 
      [[-1, 6], 1, Concat, [1]],  # cat backbone P4 
      [-1, 3, BottleneckCSP, [512, False]],  # 13 
    
      [-1, 1, Conv, [256, 1, 1]], 
      [-1, 1, nn.Upsample, [None, 2, 'nearest']], 
      [[-1, 4], 1, Concat, [1]],  # cat backbone P3 
      [-1, 3, BottleneckCSP, [256, False]],  # 17 (P3/8-small) 
    
      [-1, 1, Conv, [256, 3, 2]], 
      [[-1, 14], 1, Concat, [1]],  # cat head P4 
      [-1, 3, BottleneckCSP, [512, False]],  # 20 (P4/16-medium) 
    
      [-1, 1, Conv, [512, 3, 2]], 
      [[-1, 10], 1, Concat, [1]],  # cat head P5 
      [-1, 3, BottleneckCSP, [1024, False]],  # 23 (P5/32-large) 
    
      [[17, 20, 23], 1, Detect, [nc, anchors]],  # Detect(P3, P4, P5) 
     ] 
  ```

  

  - ### Fix this 10 layer method in yolov5/train.py

  ```
  # Freeze
  freeze = []  # parameter names to freeze (full or partial)
  ```

  

  - ### change this to:

  ```
  freeze = ['model.%s.' % x for x in range(10)]
  ```

  ###  This alone will lock the weights of the first 10 layers and will not change during training.

  ### Next article「如和使用YOLOV5训练自己的数据------How to train your data with YoloV5 」

