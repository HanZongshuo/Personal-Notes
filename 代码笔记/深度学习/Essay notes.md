
先尝试跑通这个模型 [francescoareoluci/tbr-event-object-detection: A framework to perform object detection over Temporal Binary Encoded frames](https://github.com/francescoareoluci/tbr-event-object-detection?tab=readme-ov-file)
然后这里有其余的结构[HEHEHE_HEHHEH](https://www.kaggle.com/code/karankumawat17/hehehe-hehheh)
换骨干可以使用[Models and pre-trained weights — Torchvision main documentation](https://docs.pytorch.org/vision/main/models) 这里提供的

## 1.数据集
在train训练集中有行人的文件有387个，仅包含汽车的文件数为1173，子集构造原则考虑数据的平衡，行人和汽车的数量在1：3-1:5左右，保证行人的数量，以及一些纯汽车文件来覆盖不同的场景类别，由于行人出现的背景较为固定，如人行道，路口。避免假阳性，即在人行道前模型检测出行人这类的结果。同时保留一些纯汽车文件，以涵盖高速公路，隧道等场景。

训练集：
原始总文件数: 1458
采样后文件数: 294 (20.2%)
行人文件: 198
纯汽车文件: 96
行人实例: 7465
汽车实例: 32232
总体比例 (行人:汽车): 1:4.32

现在的比例保证了模型可以看到汽车，也不会因为汽车太多而忽略行人。这里提高行人文件的目的是为了让模型学会分辨出行人。
按照8：1:1的比例，选取测试集以及验证集。即测试集和验证集的数量在40左右。

测试集：
总文件数: 40
  行人文件: 17
  汽车文件: 23
总行人实例: 1447
总汽车实例: 6598
总体比例 (行人:汽车): 1:4.56          测试集中纯汽车文件数大于含行人文件数，防止模型训练时偷懒，并且比例与训练集比例接近。

验证集：选取40个文件，其中。由于汽车与行人数量不均衡，采集40个样本为了保证总体实例比例在1：4左右，选取含人最多的文件以及车辆最少的文件。
总文件数: 40
  行人文件: 35
  汽车文件: 5
总行人实例: 1175
总汽车实例: 6202
总体比例 (行人:汽车): 1:5.28

## 2 模型运行结果

### YOLOv3-Tiny
在yolo-tiny模型中，跑100个epoch，在第12的时候断开了。由于先前的模型是保存每一个epoch下的模型，我改成了保存最优epoch以及最新的epoch来减小存储压力，现在代码中采取了“兼容模式”来加载旧的权重，因此没有包含优化器（Adam）状态，导致断点续训的开始阶段重新积累动量，指标出现震荡

可调整代码让tensorboard输出graph图像。然后记得修改train代码中的兼容逻辑。

启动代码类似于
 E:\Project\Thesis\outer_source\tbr-event-object-detection\PyTorch-YOLOv3>python train.py --model_def ../yolo_config/yolov3-tiny.cfg --data_config ../yolo_config/gen1.data


> [!NOTE] 启动代码
> set KMP_DUPLICATE_LIB_OK=TRUE
> python train.py --model_def ../yolo_config/yolov3-tiny.cfg --data_config ../yolo_config/gen1.data --pretrained_weights checkpoints/yolov3_last.pth



> [!NOTE] 虚拟机启动：
> cd /root/autodl-tmp/yolov3/tbr-event-object-detection/PyTorch-YOLOv3
	python train.py --model_def config/yolov3.cfg --data_config ../yolo_config/gen1.data --verbose 2>&1 | tee train_error.log
	虚拟机账户  ssh -p 42373 root@connect.nmb2.seetacloud.com
    密码  a3twpgVyjsA2

tensorboard代码：tensorboard --logdir=logs
![[Pasted image 20251202093206.png]]

推理启动代码python test_gen1.py --model_def ../yolo_config/yolov3-tiny.cfg --data_config ../yolo_config/gen1-test.data --weights_path ../PyTorch-YOLOv3/checkpoints/yolo-tiny/yolov3_best.pth --gen1_output ../inference_res_low_conf --total_acc_time 2500 --n_cpu 0  --conf_thres 0.01#这里可以确定置信度


训练结果
| Index | Class name | AP      |
+-------+------------+---------+
| 0     | vehicle    | 0.25477 |
| 1     | pedestrian | 0.37227 |
+-------+------------+---------+
---- mAP 0.3135213885170083
---- Saved best model with mAP 0.31352

推理结果
Class precisions:
+ Class '0' (vehicle) - Precision: 0.46045154354169865
+ Class '1' (pedestrian) - Precision: 0.3089041095890411
Class recalls:
+ Class '0' (vehicle) - Recall: 0.4739171672462852
+ Class '1' (pedestrian) - Recall: 0.3118948824343015
Average Precisions:
+ Class '0' (vehicle) - AP: 0.3809615776021013
+ Class '1' (pedestrian) - AP: 0.1893727355549985
mAP: 0.2851671565785499

在低置信度0.01下，
Class precisions:
+ Class '0' (vehicle) - Precision: 0.02608200995496563
+ Class '1' (pedestrian) - Precision: 0.020038543223472035
Class recalls:
+ Class '0' (vehicle) - Recall: 0.8697439140056908
+ Class '1' (pedestrian) - Recall: 0.7047026279391425
Average Precisions:
+ Class '0' (vehicle) - AP: 0.4593693217908154
+ Class '1' (pedestrian) - AP: 0.27242476030104684
mAP: 0.3658970410459311

置信度0.15下
Class precisions:
+ Class '0' (vehicle) - Precision: 0.13110515528929528
+ Class '1' (pedestrian) - Precision: 0.07877507919746568
Class recalls:
+ Class '0' (vehicle) - Recall: 0.6572873853936136
+ Class '1' (pedestrian) - Recall: 0.5159059474412172
Average Precisions:
+ Class '0' (vehicle) - AP: 0.4330794804988046
+ Class '1' (pedestrian) - AP: 0.2274683651608997
mAP: 0.33027392282985213

### yolov3
One-Stage Object Detection 是直接在神经网络中定位和分类的视觉方法。与传统的双阶段检测区别在于速度和流程。主要优势在于实时性（YOLO）；双阶段的检测器在于精度高R-CNN, Fast R-CNN, **Faster R-CNN**, Mask R-CNN。[YOLOv4 Explained | CIOU Loss, CSPDarknet53, SPP, PANet | Everything about it](https://www.youtube.com/watch?v=b148nt9P8J0&t=19s) 

mAP， PR曲线，FPS
![[mAPcompare_yolov3_vs_tiny.jpg]]
尽管在训练阶段通过人工干预提高了验证集的难度，导致验证 mAP 处于较低水平（<0.4），但在标准测试集上的推理结果（0.432）证明了模型具有较强的泛化性能，并未产生过拟合。这同时也暗示了在 2500μs 的极短累计时间下，模型依然能够捕捉到关键的事件流特征。
Yolo v3:
Class precisions:
+ Class '0' (vehicle) - Precision: 0.36527001008526633
+ Class '1' (pedestrian) - Precision: 0.19627998042094957
Class recalls:
+ Class '0' (vehicle) - Recall: 0.6297818526715144
+ Class '1' (pedestrian) - Recall: 0.5546334716459198
Average Precisions:
+ Class '0' (vehicle) - AP: 0.5104082855187513
+ Class '1' (pedestrian) - AP: 0.3541100627995024
mAP: 0.43225917415912685

Inference Statistics:  来自RTX3060，cu12.1
Average Inference Time (Model + NMS): 0.0165 s
Average FPS: 60.44




This is based on TBR enoder, accumulation_time=2.5ms
累计成帧启动指令python rt_detection.py --event_video "F:\dataset\val_a\detection_dataset_duration_60s_ratio_1.0\val\17-04-06_18-48-51_1098500000_1158500000_td.dat" --weights_path "E:\Project\Thesis\outer_source\tbr-event-object-detection\PyTorch-YOLOv3\checkpoints\yolov3\yolov3_best.pth" --class_path "D:\converted\data\custom\classes.names" --accumulation_time 2500 --show_video
![[Pasted image 20251222230517.png]]把事件相机累计成帧，丢失了事件相机的时间稀疏性优势


### yolov8-nano

`/scratch/project_2017150/dataset module load pytorch pip install ultralytics`
 Class     Images  Instances      Box(P          R      mAP50  mAP50-95): 
    all       3055       7638      0.639      0.494      0.535      0.277
 vehicle       2997       6195      0.673      0.549      0.598      0.351
pedestrian        472       1443      0.604      0.439      0.473      0.204
Speed: 0.7ms preprocess, 2.2ms inference, 0.0ms loss, 0.7ms postprocess per image
Results saved to E:\Project\Thesis\yolov8\runs\detect\val
Class precisions:
+ Class '0' (vehicle) - Precision: 0.6729919497011042
+ Class '1' (pedestrian) - Precision: 0.6041011617000577

Class recalls:
+ Class '0' (vehicle) - Recall: 0.5494753833736885
+ Class '1' (pedestrian) - Recall: 0.4386694386694387

Average Precisions (AP@50):
+ Class '0' (vehicle) - AP: 0.5981719759047638
+ Class '1' (pedestrian) - AP: 0.4727150895916859

Overall Performance:
mAP@50: 0.5354435327482249
mAP@50-95: 0.27736086104524926

Speed Metrics:
Pre-process: 0.71 ms
Inference: 2.15 ms
Post-process: 0.65 ms
Total time per image: 3.51 ms
FPS: 284.69

==============================


### yolov8
Class     Images  Instances      Box(P          R      mAP50  mAP50-95): 
 all       3055       7638      0.714      0.528       0.58      0.297
 vehicle       2997       6195      0.729      0.569      0.635      0.379
pedestrian        472       1443        0.7      0.486      0.524      0.215
Speed: 1.0ms preprocess, 32.7ms inference, 0.0ms loss, 0.6ms postprocess per image
Results saved to E:\Project\Thesis\yolov8\runs\detect\val4

Class precisions:
+ Class '0' (vehicle) - Precision: 0.728727554177611
+ Class '1' (pedestrian) - Precision: 0.6996971585293542

Class recalls:
+ Class '0' (vehicle) - Recall: 0.5690072639225182
+ Class '1' (pedestrian) - Recall: 0.4860164489929669

Average Precisions (AP@50):
+ Class '0' (vehicle) - AP: 0.6354711889739352
+ Class '1' (pedestrian) - AP: 0.5239621290249783

Overall Performance:
mAP@50: 0.5797166589994567
mAP@50-95: 0.29681944521448805

Speed Metrics:
Pre-process: 0.71 ms
Inference: 32.00 ms
Post-process: 0.62 ms
Total time per image: 33.34 ms
FPS: 30.00

accumulate time 2.5 ms  confidence threshold 0.5

| **Model**   | **mAP50​** | **mAP50−95​** | **Inference Time (ms)** | **FPS** |
| ----------- | ---------- | ------------- | ----------------------- | ------- |
| YOLOv3-tiny | 0.311      | 0.122         | 2.61                    | 383.0   |
| YOLOv3      | 0.432      | 0.171         | 12.15                   | 82.3    |
| YOLOv8n     | 0.535      | 0.277         | 3.5                     | 284.7   |
| YOLOv8x     | 0.580      | 0.297         | 33.3                    | 30.0    |

| Model       | **Class**  | **Precision (P)** | **Recall (R)** | **AP (@50)** |
| ----------- | ---------- | ----------------- | -------------- | ------------ |
| YOLOv3      | Vehicle    | 0.365             | **0.630**      | 0.510        |
|             | Pedestrian | 0.196             | **0.555**      | 0.354        |
| YOLOv3-tiny | Vehicle    | 0.432             | 0.512          | 0.398        |
|             | Pedestrian | 0.311             | 0.355          | 0.223        |
| YOLOv8x     | Vehicle    | **0.729**         | 0.569          | **0.635**    |
|             | Pedestrian | **0.700**         | 0.486          | **0.524**    |
| YOLOv8n     | Vehicle    | 0.673             | 0.549          | 0.598        |
|             | Pedestrian | 0.604             | 0.439          | 0.473        |

---
accumulate time 30ms

| **Model**    | **mAP50​** | **mAP50−95​** | **Inference Time (ms)** | **FPS** |
| ------------ | ---------- | ------------- | ----------------------- | ------- |
| yolov8n(TBR) | 0.665      | 0.358         | 2.1                     | 285.3   |
| yolov8n(SAE) | 0.669      | 0.363         | 2.04                    | 285.97  |
| yolov8s(SAE) | 0.681      | 0.367         | 4.8                     | 158.8   |
| yolov8m(SAE) | 0.701      | 0.381         | 11.8                    | 75.12   |

| Model        | **Class**  | **Precision (P)** | **Recall (R)** | **AP (@50)** |
| ------------ | ---------- | ----------------- | -------------- | ------------ |
| yolov8n(TBR) | Vehicle    | 0.819             | 0.688          | 0.756        |
|              | Pedestrain | 0.640             | 0.523          | 0.575        |
| yolov8n(SAE) | Vehicle    | 0.783             | 0.655          | 0.720        |
|              | Pedestrain | 0.707             | 0.583          | 0.620        |
| yolov8s(SAE) | Vehicle    | 0.801             | 0.640          | 0.722        |
|              | Pedestrain | 0.739             | 0.592          | 0.640        |
| yolov8m(SAE) | Vehicle    | 0.799             | 0.659          | 0.737        |
|              | Pedestrain | 0.751             | 0.615          | 0.665        |
![[BoxF1_curve 1.png]]
![[BoxF1_curve.png]]

如果去掉重复的检测框，得到的结果采用yolov8n_30_sae下

| Model        | **Class**  | **Precision (P)** | **Recall (R)**          | **AP (@50)** |
| ------------ | ---------- | ----------------- | ----------------------- | ------------ |
| yolov8n(SAE) | Vehicle    | 0.784             | 0.676                   | 0.742        |
|              | Pedestrain | 0.707             | 0.581                   | 0.620        |
|              |            |                   |                         |              |
| **Model**    | **mAP50​** | **mAP50−95​**     | **Inference Time (ms)** | **FPS**      |
| yolov8n(SAE) | 0.681      | 0.371             | 2.26                    | 251          |


## 论文内容
"The significant difference in experimental results demonstrates that for sparse spatiotemporal data generated by event cameras, traditional anchor-based methods (such as YOLOv3) struggle to adapt to the varied shapes of pedestrian targets due to their geometric dependence on predefined bounding boxes.

In contrast, YOLOv8's anchor-free strategy, combined with a Task Aligned Assigner, eliminates the constraints of rigid anchor boxes and effectively separates spatial localization and semantic classification tasks using decoupled heads. This explains why, even under training from scratch conditions, YOLOv8 still exhibits significantly higher feature extraction efficiency and generalization ability than YOLOv3."

在2.5ms的累计下，无法做到真正的实时监测，因为推理以及处理速度已经有3ms左右，因此需要提升累计时间来提高识别率。
设计两部分的消融实验：
1. 数据表征与稀疏度的消融实验：验证30ms累计下的结果，证明充足的事件数据是识别基础（30ms类似于帧相机的33FPS帧率，在长的累计时间会导致事件帧出现严重的拉伸）然后发现行人检测结果不尽人意，因为行人较小，手脚摆动容易糊在一起，而汽车体型大，速度快，抗噪声能力更强。
2. 在最佳累计试验下，对比Polarity或者SAE来证明更高级的表征方式能够更好抑制噪声并突出边缘


#### 1. 消融实验 (Ablation Study) —— 论文的“骨架”
你需要用数据回答几个“为什么”：
- **时间窗口消融**：展示 SAE 在 10ms, 20ms, 30ms, 40ms 下的性能变化。证明 30ms 确实是“甜点位（Sweet Spot）”。
    
- **编码方式消融**：固定模型为 YOLOv8n，横向对比 TBR vs. SAE vs. 原始事件流（如果可能）。
#### 2. 复杂场景分析 (Scenario Analysis) —— 论文的“血肉”

不要只看总分。将测试集分类：
- **高速 vs. 低速**：分析 SAE 在不同车速下的表现。SAE 理论上在高速下比 TBR 优势更明显。
- **日间 vs. 夜间**：事件相机在低光下有优势，展示 SAE 在夜间场景下如何保持行人检测的稳定性。
#### 3. 可视化分析 (Feature Visualization) —— 论文的“门面”
使用 **Grad-CAM**（梯度加权类激活映射）生成热力图。
- 展示模型在看 TBR 图像时注意力是涣散的（模糊区域），而看 SAE 图像时注意力精准地集中在目标的**前导边缘**。这种可视化比表格更有说服力。
#### 4. 效率与性能的权衡 (Trade-off Analysis)
做一个 **FPS vs. mAP** 的散点图。
- 标注出你的 YOLOv8n + SAE 处于图表的左上角（既快又准）。
- 强调你的方案在 CPU/显存占用极低的情况下，达到了比肩大模型（如 YOLOv8x + TBR）的精度。这在自动驾驶的嵌入式端具有巨大的**商业和工程价值**。