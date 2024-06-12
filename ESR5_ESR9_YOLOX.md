### Knowledge Distillation in YOLOX-ViT for Side-Scan Sonar Object Detection

#### Introduction
Exploring the oceanic environment has become increasingly important due to various underwater activities such as infrastructure development and archaeological explorations. Autonomous underwater vehicles (AUVs) play a crucial role in these tasks, enabling efficient data collection and underwater operations. However, the complex nature of underwater environments demands advanced decision-making capabilities and high situational awareness. 

In this context, deep learning (DL) based computer vision offers a promising solution for real-time detection. Yet, the large size of standard DL models poses challenges for AUVs concerning power consumption, memory allocation, and real-time processing needs. This paper introduces YOLOX-ViT, an advanced object detection model incorporating a vision transformer layer and utilizing knowledge distillation to reduce model size without sacrificing performance.

#### YOLOX-ViT Model
YOLOX-ViT enhances the YOLOX model by integrating a vision transformer layer (ViT) between the backbone and neck, significantly improving feature extraction capabilities. The ViT layer is configured with 4 multi-head self-attention (MHSA) sub-layers, combining the spatial hierarchy of CNNs with the global context of transformers. This integration enhances the model's ability to detect objects in complex underwater environments.

#### Knowledge Distillation
Knowledge distillation (KD) is employed to transfer knowledge from a larger, well-trained model (teacher) to a smaller model (student), using a combined loss function that incorporates both hard and soft loss components. This process allows the smaller model to learn from the nuanced behaviors of the larger model, improving its performance while maintaining a reduced size.

The KD process in YOLOX-ViT involves computing distinct loss functions for each output of the feature pyramid network (FPN), ensuring a comprehensive transfer of knowledge across classification, bounding box regression, and objectness scores. Both online and offline KD methods are explored, with the offline method reducing training time significantly.

#### Sonar Wall Detection Dataset (SWDD)
A new side-scan sonar (SSS) image dataset, the Sonar Wall Detection Dataset (SWDD), is introduced. Collected in Porto de Leixões harbor using a Klein 3500 sonar on a light autonomous underwater vehicle (LAUV), the dataset includes 216 images along harbor walls. Data augmentation techniques such as noise addition, horizontal flips, and combined transformations are used to enhance the dataset's robustness.

The dataset features two classes, "Wall" and "NoWall," with 2,616 labeled samples. The images are scaled to 640 × 640 for compatibility with computer vision algorithms, and the dataset is publicly accessible for research purposes.

#### Experimental Evaluation
The experimental evaluation of YOLOX-ViT involves training and validating the model on real-world data, including a detailed video analysis. Metrics such as true positives (TP), false positives (FP), precision (Pr), average precision at 50% IoU (AP50), and overall average precision (AP) are used to assess performance. The results demonstrate the effectiveness of YOLOX-ViT and the KD approach in reducing false positives and improving detection accuracy in underwater environments.

#### Conclusion
The integration of a visual transformer layer and the application of knowledge distillation in YOLOX-ViT provide a significant advancement in object detection for underwater robotics. The model achieves high accuracy with a reduced size, making it suitable for real-time implementation on AUVs. The introduced Sonar Wall Detection Dataset (SWDD) further supports research in this domain, offering valuable data for future studies.

The source code for knowledge distillation in YOLOX-ViT is available at [https://github.com/remaro-network/KD-YOLOX-ViT](https://github.com/remaro-network/KD-YOLOX
