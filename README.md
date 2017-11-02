# Create python environment
```
conda env create -f environment.yml
source activate sushi_detector
```

# Create TFRecord
[TBD]

# Download pretrained model
```
pushd data
pretrained_model=ssd_mobilenet_v1_coco_11_06_2017
curl -OL http://download.tensorflow.org/models/object_detection/${ssd_mobilenet_v1_coco_11_06_2017}.tar.gz
tar -xzf ${pretrained_model}.tar.gz
rm -rf ${pretrained_model}.tar.gz ${pretrained_model}
popd
```

# Running in docker container
## Training
```
docker run -d -v `pwd`/data:/data --name sushi_detector_train jwata/tensorflow-object-detection \
  python object_detection/train.py \
  --logtostderr \
  --pipeline_config_path=/data/ssd_mobilenet_v1_sushi.docker.config \
  --train_dir=/data/train
```

## Evaluation
```
docker run -d -v `pwd`/data:/data --name sushi_detector_eval jwata/tensorflow-object-detection \
  python object_detection/eval.py \
  --logtostderr \
  --pipeline_config_path=/data/ssd_mobilenet_v1_sushi.docker.config \
  --checkpoint_dir=/data/train \
  --eval_dir=/data/eval
```

## Tensorboard
```
docker run -d -v `pwd`/data:/data -p 6006:6006 --name tensorboard jwata/tensorflow-object-detection \
  tensorboard --logdir=/data

open http://localhost:6006
```
