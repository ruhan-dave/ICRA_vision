# ICRA Computer Vision Project


# Agriculural Application of Computer Vision (Object Detection and Counting)

🌱 A client of ICRA needed an automatic process of counting agricultural produce to save time, operational costs, and improve accuracy. 

## Environment Variables

1. You will need a Colab Pro subscription to access Google's GPU. 
2. To annotate images and create the image files required for the model training process, you will need to log in to your Roboflow Account, select the relevant project, and annotate.
 
## Documentation of Project Workflow

1. You will need to log into Roboflow.
`login`
<img width="1411" alt="Screen Shot 2022-09-09 at 3 33 25 PM" src="https://user-images.githubusercontent.com/63888029/189458016-fe5cb3f1-2f7a-4c82-a1a2-3f01cc05ce5d.png">
`go to the project called improve`
<img width="1403" alt="Screen Shot 2022-09-09 at 3 33 41 PM" src="https://user-images.githubusercontent.com/63888029/189458027-98454317-ae1d-44ce-ac49-89a3bb008b87.png">
`upload images`
<img width="1415" alt="Screen Shot 2022-09-09 at 3 33 58 PM" src="https://user-images.githubusercontent.com/63888029/189458036-cdab08da-2563-45c7-864a-10057b1b7bac.png">
`annotate in the annotation tool box`
<img width="1427" alt="Screen Shot 2022-09-09 at 3 35 02 PM" src="https://user-images.githubusercontent.com/63888029/189458482-f7e61407-70f4-4d95-9896-f2101dce4feb.png">
`select the Polygon option (3rd), and then zoom in to annotate`
<img width="1075" alt="Screen Shot 2022-09-09 at 3 35 44 PM" src="https://user-images.githubusercontent.com/63888029/189458487-3202a973-a15f-4db1-b7f4-132e17e45c9b.png">
`click "generate" in the left menu bar, then choose the data augmentations`
<img width="1308" alt="Screen Shot 2022-09-09 at 3 36 24 PM" src="https://user-images.githubusercontent.com/63888029/189458506-b152ed49-1fff-4620-a329-d43905fc6dfc.png">
`click "generate"`
<img width="1416" alt="Screen Shot 2022-09-09 at 3 36 44 PM" src="https://user-images.githubusercontent.com/63888029/189458512-3154bfa6-a22a-4234-a679-675a1f7fc0fc.png">
`Select "COCO" format and then choose to download to computer (your own computer)`
<img width="859" alt="Screen Shot 2022-09-09 at 3 37 19 PM" src="https://user-images.githubusercontent.com/63888029/189458516-3b4fd658-6ffb-466d-8977-5f1159bc43dc.png">
`It will be downloaded as a zip file`
And contains all the json files and image files for both the `train` and the `test` sets. Please follow the notebook guideline closely.
<img width="388" alt="Screen Shot 2022-09-09 at 3 37 25 PM" src="https://user-images.githubusercontent.com/63888029/189458532-9a6d5038-c5e1-4f46-bbcb-a746ef151eb4.png">

## Run Locally

To run this project, you will need to access and run the 
`ICRA_TFOD_Segmentation.ipynb` which contains all the relevant code for inferencing.

If you need to train a model from scratch, you will need to run the `!{command}` line in the colab notebook. 
All code contains clearly commented instructions that you can easily follow.

## Screenshots

Annotating images by drawing boundaries of each object of interest (masks):

<img width="354" alt="Screen Shot 2022-09-02 at 10 54 03 AM" src="https://user-images.githubusercontent.com/63888029/189072054-734d97ed-2d32-4df2-bb9e-79ac53df4577.png">

## Model Used and Output

The model I used was a pre-trained model from the TensorFlow Library Object Detection API model zoo (Mask R-CNN Inception ResNet V2 1024x1024) from the  [tensorflow model zoo] (https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/tf2_detection_zoo.md). This model is one of the slowest models in the entire faily but is one of the more accurate models. The reason why this model was picked was that it is the only one that could ouput masks. However, it is the only model that allows for drawing boundary masks over the objects.

From the Tensorflow Model Zoo:

| Model         | Name          | Speed (ms)|  COCO mAP  |   Outputs     |
| ------------- |:-------------:| -----:    | ------: | ------: | -----: |
| Mask R-CNN      | Inception ResNet V2 1024x1024|   301   |   39.0/34.6     |   Boxes/Masks   |  

There are 2 classes associated: 0 or 1, which makes this problem akin to binary classification. 0 means the background and 1 the agricultural produce (flower). This classification is applied to every pixel of the image, and those identifiedas 1 would have a mask drawn over. Thus, the number of flowers is nothing but the total number of masks drawn over all groups of pixels identified as 1 or the flower. 

Training process:

<img width="1004" alt="demo_training" src="https://user-images.githubusercontent.com/63888029/189071949-7c18f3b8-46be-4110-b8ce-e69c011e5a05.png">

Demo of results:

<img width="1032" alt="demo_result" src="https://user-images.githubusercontent.com/63888029/189071997-3e1956a0-e3da-4447-9e2e-4874341c9886.png">

## Results

<img width="544" alt="Screen Shot 2022-09-08 at 1 32 50 AM" src="https://user-images.githubusercontent.com/63888029/189075265-2d3103dc-57cd-475e-9c42-bbed3cac0c7f.png">

<img width="570" alt="best" src="https://user-images.githubusercontent.com/63888029/189075449-25fae0f2-79e5-486d-8d56-c400841c0023.png">

## Metrics Used

The metrics used were: accuracy, precision, recall, and F-1 score. One note is that for precision (measuring % of model-predicted positives were actually positives), the number is almost always 1 (the highest possible value). The reason for that is, we are more interested in the postives (not the negatives, which could be defined as the background or the cameraman's feet or perhaps a rock or a sign?) Meanwhile, recall is actually what we care about the most, since it measures the percentage of correctly identified flowers. I also included F-1 for completion. 

## Recommendations for Further Model Improvement

To improve the accurareliability, I recommend doing the same as I had done: selecting a variety of images with flowers of different shapes, sizes, color saturations, brightness, etc. This ensures a diverse image dataset to train the model with.

Meanwhile, another tip that can help a lot:
Select images with flowers overlapping each other or have a good amount of flowers around the corners of the image (please annotate them, even if these flowers may be partially cut off)

This tip above will help eliminate some of the false negatives that the model failed to identify as positive.

## Deployment

This project is in progress, as newer versions will be created. 

Suggestion is to deploy it on Amazon AWS Sagemaker to ensure scalability and use across the internet.
