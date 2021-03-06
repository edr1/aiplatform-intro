# Introduction to Google AI Platform
This file contains text you can copy and paste for the examples in Cloud Academy's _Introduction to Google AI Platform_ course.  

### Introduction
[Google Cloud Platform Free Trial](https://cloud.google.com/free)  

### TensorFlow
[TensorFlow website](https://www.tensorflow.org)  
[TensorFlow installation](https://www.tensorflow.org/install/pip)  

```
python3 -V      # Check which version of Python 3 is installed
pip3 install --user --upgrade pip
pip3 install --user --upgrade virtualenv
virtualenv mlenv
source mlenv/bin/activate
pip3 install tensorflow==2.2
```

```
git clone https://github.com/cloudacademy/aiplatform-intro.git
cd aiplatform-intro/iris/trainer
python3 iris.py --job-dir export
```

### Training a Model with AI Platform
[Google Cloud SDK installation](https://cloud.google.com/sdk)  

```
cd ..
gcloud ai-platform local train --module-name trainer.iris --package-path trainer --job-dir export
```
```
PROJECT=$(gcloud config list project --format "value(core.project)")
BUCKET=gs://${PROJECT}-aiplatform
REGION=us-central1
gsutil mb -l $REGION $BUCKET
```
[AI Platform Training Regions](https://cloud.google.com/ai-platform/training/docs/regions)  
```
JOB=iris1
gcloud ai-platform jobs submit training $JOB \
    --module-name trainer.iris \
    --package-path trainer \
    --staging-bucket $BUCKET \
    --region $REGION \
    --python-version 3.7 \
    --runtime-version 2.2 \
    --job-dir $BUCKET/$JOB
```
[Runtime version list](https://cloud.google.com/ai-platform/training/docs/runtime-version-list)  

### Feature Engineering
[PetFinder dataset columns](https://www.tensorflow.org/tutorials/structured_data/feature_columns#the_dataset)  

```
pip3 install numpy pandas sklearn
cd ../pets
```
```
gcloud ai-platform local train --module-name trainer.pets --package-path trainer --job-dir export
```

### Distributed Training on AI Platform
```
JOB=pets1
gcloud ai-platform jobs submit training $JOB \
    --module-name trainer.pets \
    --package-path trainer \
    --staging-bucket $BUCKET \
    --region $REGION \
    --python-version 3.7 \
    --runtime-version 2.2 \
    --scale-tier STANDARD_1 \
    --job-dir $BUCKET/$JOB
```

### Deploying a Model on AI Platform
```
cd ../iris
gcloud ai-platform models create iris --regions=$REGION
```
```
gcloud ai-platform versions create v1 \
    --model iris \
    --runtime-version 2.2 \
    --region global \
    --origin $BUCKET/iris1
```
```
gcloud ai-platform predict \
    --model iris \
    --version v1 \
    --region global \
    --json-request test.json
```

### Summary
[AI Platform documentation](https://cloud.google.com/ai-platform/docs)  
support@cloudacademy.com  

