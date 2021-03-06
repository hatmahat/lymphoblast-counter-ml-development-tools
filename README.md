# Our Machine Learning Workflow 
Model baseline based on DeepLearning.AI: Data and Deployment Course (object detection model trained on the coco)  
![workflow](workflow.png)  
TensorFlow Object Detection API Link: https://github.com/tensorflow/models/tree/master/research/object_detection
## 1. Data acquisition from Acute Lymphoblastic Leukemia Image Database for Image Processing
- Public dataset link: https://homes.di.unimi.it/scotti/all/

## 2. Image preprocessing
- Labeling Image with LabelImg: https://github.com/tzutalin/labelImg  
  
![labelimg-documentation](labelimg-documentation.jpg)

## 3. Augmentation, resize and spliting data
- Data augmentation and resize using Roboflow: https://roboflow.com/
- Roboflow Train/Test split details (90% train, 10% test):  
  
![roboflow-documentation-1](roboflow-documentation-1.jpg)
- New dataset output:  
  
![roboflow-documentation-2](roboflow-documentation-2.jpg)

## 4. Configure pre-trained model based on your choosing
- Choose the pre-trained model from TensorFlow 2 Detection Model Zoo: https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/tf2_detection_zoo.md
- Configure pipeline.config from the model you downloaded
- Read pipeline.config from CONFIG_PATH
```
pipeline_config = pipeline_pb2.TrainEvalPipelineConfig()
with tf.io.gfile.GFile(CONFIG_PATH, "r") as f:
      proto_str = f.read()
      text_format.Merge(proto_str, pipeline_config)
```
- Configure pipeline.config as required by the model you choosed (this is just an example that i used)
```
pipeline_config.model.ssd.num_classes = 2
pipeline_config.train_config.batch_size = 4
pipeline_config.train_config.fine_tune_checkpoint = PRETRAINED_MODEL_PATH+'/ssd_resnet50_v1_fpn_640x640_coco17_tpu-8/checkpoint/ckpt-0'
pipeline_config.train_config.fine_tune_checkpoint_type = "detection"
pipeline_config.train_input_reader.label_map_path= ANNOTATION_PATH + '/label_map.pbtxt'
pipeline_config.train_input_reader.tf_record_input_reader.input_path[:] = [ANNOTATION_PATH + '/train.record']
pipeline_config.eval_input_reader[0].label_map_path = ANNOTATION_PATH + '/label_map.pbtxt'
pipeline_config.eval_input_reader[0].tf_record_input_reader.input_path[:] = [ANNOTATION_PATH + '/test.record']
```
- Write back to pipeline.config that you already configured
```
config_text = text_format.MessageToString(pipeline_config)
with tf.io.gfile.GFile(CONFIG_PATH, "wb") as f:
     f.write(config_text)
```

## 5. Train the model
- Use this command to train your model and change the directory and num_training_steps as you required (strongly recommend using GPU)
```
python Tensorflow/models/research/object_detection/model_main_tf2.py --model_dir=Tensorflow/workspace/models/my_ssd_resnet50 --pipeline_config_path=Tensorflow/workspace/models/my_ssd_resnet50/pipeline.config --num_train_steps=5000
```

## 6. Evaluate the model performance
- Use this command to evalate using tensorboard:
```
tensorboard --logdir=Tensorflow\workspace\models\my_ssd_resnet50
```
- Use this command to see your COCO Metrics Results from command prompt
```
python Tensorflow\models\research\object_detection\model_main_tf2.py --model_dir=Tensorflow\workspace\models\my_ssd_resnet50 --pipeline_config_path=Tensorflow\workspace\models\my_ssd_resnet50\pipeline.config --checkpoint_dir=Tensorflow\workspace\models\my_ssd_resnet50
```

## 7. Restore checkpoint 
- Restore checkpoint, change the ckpt-59 with your final checkpoint from CHECKPOINT_PATH
```
# Restore checkpoint (last checkpoint)
ckpt = tf.compat.v2.train.Checkpoint(model=detection_model)
ckpt.restore(os.path.join(CHECKPOINT_PATH, 'ckpt-59')).expect_partial()
```
## 8. Detection of lymphoblast and non-lymphoblast 
- To detect lymphoblast and non-lymphoblast, change the "img_22.jpg" with image you want to detect (remember to use 640x640 image size)
```
img = cv2.imread("img_22.jpg")
```
## 9. Example output image
- Example output 1:  
  
![output1](output1.jpg)
- Example output 2:  
  
![output2](output2.jpg)
- Example output 3:  
  
![output3](output3.jpg)

### if instructions are not clear enough, please email me: mahatma.ageng.wisesa@mail.ugm.ac.id  
