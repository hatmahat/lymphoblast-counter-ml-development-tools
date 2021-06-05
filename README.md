### Machine Learning Workflow:
![workflow](workflow.png)

## 1) Data Acquisition from Acute Lymphoblastic Leukemia Image Database for Image Processing
- Public dataset link: https://homes.di.unimi.it/scotti/all/

## 2) Image Preprocessing
- Labeling Image with LabelImg: https://github.com/tzutalin/labelImg
![labelimg-documentation](labelimg-documentation.jpg)

## 3) Augmentation, Resize and Spliting Data
- Data augmentation and resize using Roboflow: https://roboflow.com/
- Roboflow Train/Test split details (90% train, 10% test):
![roboflow-documentation-1](roboflow-documentation-1.jpg)
- New dataset output:
![roboflow-documentation-2](roboflow-documentation-2.jpg)

## 4) Build and Train Model
- Choose model from TensorFlow 2 Detection Model Zoo: https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/tf2_detection_zoo.md
- Configure pipeline.config  gunakan bla2

