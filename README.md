# Barnacle Counter via Image Segmentation

## Overview
### Problem Statement
Scientists with the National Park Service are researching barnacle populations in coastal tide pools on the east coast. To analyze the results of their experiments, they often need to count the number of barnacles in a given area. To do this, they place a fixed size frame on a barnacle-covered rock, then take a picture. Later, a scientist/lab tech will manually count the number of barnacles in the picture, then record their results. There are often upwards of 1000 barnacles in an image, so this is a very time consuming process. 


This project estimates the number of barnacles in coastal grid images using deep learning-based image segmentation through the following steps.

---

## Dataset
The given dataset consists of high-resolution coastal images of barnacles alongside their respective binary masks identifying barnacle regions. 

A secondary dataset was generated, consisting of cropped raw images and masks, isolating the central green-wired frame to decrease noise in model training. 

---

## Pipeline

### 1. Visualize Images
- Raw RGB images and masks are loaded into OpenCV and visualized
- Overlays of masks on raw images are used for verification

### 2. Preprocessing / New Data Generation
- A manual point-click tool allows users to select the 4 corners of the inner green grid
- Perspective warping takes the 4 points the user clicked and extracts a square crop of the region for the image and corresponding mask
- The cropped image and mask are resized to 1000×1000 for ease of model training

### 3. Model Definition
- A custom `BarnacleDataset` class loads images and masks
- A UNet model from `segmentation_models_pytorch` is used for binary segmentation

### 4. Training
- Binary Cross-Entropy loss + Sigmoid is used
- Trained using raw images and then cropped images using two separately trained models 
- Training loss is plotted across epochs for both trainings

### 5. Prediction & Counting
- Model predicts barnacle masks on unseen images
- Masks are then binarized using a subjective threshold (e.g., 0.65)
- Barnacle count for raw image is estimated using 'cv2.connectedComponents' with area filtering (e.g., min_area = 20)

---

## Example Results

### Image 1:
- Raw barnacle count estimate: **~2604**

### Image 2:
- Raw barnacle count estimate: **~114**

---

## Requirements
- Python 3.9+
- OpenCV
- PyTorch
- segmentation-models-pytorch
- matplotlib
- NumPy
