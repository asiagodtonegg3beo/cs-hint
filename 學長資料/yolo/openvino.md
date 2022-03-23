### Intel OpenVINO-toolkit
協助開發電腦視覺應用的解決方案，能提供低算力的硬體加速並最佳化，主要功能是Model Optimizer(模型最佳化)以及提供推理引擎(Inference Engine)進行推論。



### 運作流程
![](assets/markdown-img-paste-20220315002919174.png)

將訓練好的model經由Model Optimizer產生 IR檔 (中界碼)，經由推理引擎讀取 IR model進行推論，使用者就可以透過 OpenVINO Toolkit 和 Inference Engine API 整合至開發應用程式

* 中界碼(Intermediate Representation):是一種資料結構，可將輸入的資料建構為一個程式，也可以將一部份或是所有輸出的程式反推回輸入資料
* 這意味著IR將會保留一些輸入資料的資訊，同時擁有更進一步註釋或是快速查詢的功能。

模型轉換流程：Darknet → TensorFlow → OpenVINO

.weights(Tensorflow) => .pb =>  xml,bin

.pt(Pytorch) => .onnx => xml,bin

目前最新版本是openVino2021.4

### Model 優化轉檔 (xml,bin)

* xml:保存神經網路(network)內的參數

![](assets/markdown-img-paste-20220315012330315.png)

* bin:保存權重(weight)的bias(偏差值)

![](assets/markdown-img-paste-20220315003406478.png)

### 安裝流程
1. 安裝OpenVINO推理引擎
2. 下載OpenVINO-YOLOV4
3. 下載yolov4.weight
4. 轉換yolov4.weights -> .pb
5. .pb轉換成供OpenVINO使用的.bin .xml檔
其中有提到如果要轉換成YOLO V4的權重文件，只能用>=openVino2021.3以上版本
6. 在OpenVINO使用推理引擎 ->測試

### yolo v3 tiny tf vs yolo v4 tiny tf
![](assets/markdown-img-paste-20220315111558831.png)
#### Specification
|      |v3 tiny|v 4tiny|
|------|-------|-------|
|TYPE             |Detection| Detection|
|GFLOPS           | 5.582   | 6.9289   |
|MParams          | 8.848   | 6.0535   |
|Source Framworks | Keras   | Keras    |

#### Accuracy
|      |v3 tiny|v4 tiny|
|------|-------|-------|
|MAP  |0.359|0.403|
|COCO map|    0.397|      |
|COCO map(0.5)| |  0.463     |
|COCO map(0.5:0.05:0.95)   |   | 0.226  |


mAP averaged over 10 IoU thresholds and is the primary challenge metric(基本挑戰指標)



#### input

|      |v3 tiny|v4 tiny|format|
|------|-------|-------|------|
|origin model|[1,416,416,3]|[1,416,416,3]|[B, H, W, C]
|converted model|[1,3,416,416]|[1,3,416,416]|[B, C, H, W]

* B : Batch size
* H : Height
* W : Width
* C : Channel , origin = RGB , converted = BGR


#### output

##### origin model

The array of detection summary info

| Name |v3 tiny|v4 tiny|format|anchor_value|
|------|-------|-------|------|------------|
|conv2d_9/BiasAdd |[1,13,13,255]|   |[B, Cx, Cy, N*85]|81,82, 135,169, 344,319|
|conv2d_12/BiasAdd|[1,26,26,255]|   |[B, Cx, Cy, N*85]|23,27, 37,58, 81,82|
|conv2d_17/BiasAdd|             |[1,13,13,255]|[B, Cx, Cy, N*85]|81,82, 135,169, 344,319|
|conv2d_20/BiasAdd|             |[1,26,26,255]|[B, Cx, Cy, N*85]|23,27, 37,58, 81,82|

* B : Batch size
* Cx,Cy : Cell index
* N : number of detection boxes for cell (每個單元的偵測框數)

Detection box format [x, y, h, w, box_score, class_no_1, …, class_no_80]

* (x,y) : 中心框相對於cell的座標
* h,w : 框的原始高度及寬度，使用指數函數乘以對應的anchors得到絕對值高度及寬度值
* box score : detection box 的可信度，介於 0~1 之間
* class_no_1, …, class_no_80 : 每個Class的機率分布，介於 0~1 之間，與 confidence value 相乘得到每個 Class 的可信度

該模型基於COCO dataset上進行訓練，dataset包含80個類別的物件

##### Converted model

The array of detection summary info

| Name |v3 tiny|v4 tiny|format|anchor_value|
|------|-------|-------|------|------------|
|conv2d_9/BiasAdd |[1,255,13,13]|   |[B, N*85, Cx, Cy]|81,82, 135,169, 344,319|
|conv2d_12/BiasAdd|[1,255,26,26]|   |[B, N*85, Cx, Cy]|23,27, 37,58, 81,82|
|conv2d_17/BiasAdd|             |[1,255,13,13]|[B, N*85, Cx, Cy]|81,82, 135,169, 344,319|
|conv2d_20/BiasAdd|             |[1,255,26,26]|[B, N*85, Cx, Cy]|23,27, 37,58, 81,82|

* B : Batch size
* Cx,Cy : Cell index
* N : number of detection boxes for cell (每個單元的偵測框數)

Detection box format [x, y, h, w, box_score, class_no_1, …, class_no_80]

* (x,y) : 中心框相對於cell的座標
* h,w : 框的原始高度及寬度，使用指數函數乘以對應的anchors得到絕對值高度及寬度值
* box score : detection box 的可信度，介於 0~1 之間
* class_no_1, …, class_no_80 : 每個Class的機率分布，介於 0~1 之間，與 confidence value 相乘得到每個 Class 的可信度

該模型基於COCO dataset上進行訓練，dataset包含80個類別的物件
