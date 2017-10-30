## Writeup Template
### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./output/car_notcar_example.png
[image2]: ./output/test_example.png
[video1]: ./project_video.mp4

## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README
 
#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Histogram of Oriented Gradients (HOG)

#### 1. Explain how (and identify where in your code) you extracted HOG features from the training images.

Extraction of HOG features were implemented in code cell [fill number](??) of the jupyter notebook [T1P5 Pipeline submit.ipynb](??). I started by reading in all the `vehicle` and `non-vehicle` images with 8968 vehicle images and 8792 non-vehicle images. I then explored different color spaces and different `skimage.hog()` parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`). It turned out that color spcae `YCrCb` yielded better porformed classifier than `RGB` space. Examplar images for each vechile and non-vechile are displayed below with `RGB` color space, HOG features using all 3 channels are also displayed using `orientations=8`, `pixels_per_cell=(8, 8)` and `cells_per_block=(2, 2)`.

![alt text][image1]

#### 2. Explain how you settled on your final choice of HOG parameters.

Different combinations of `orientation`, `pixels_per_cell`, and `cells_per_block` were experimented. In consideration of both classifier's performance and the time it took for training and prediction, I ended up with using HOG prameters of `orientations=8`, `pixels_per_cell =8`, and `cells_per_block=2`

#### 3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

I used grid search to test different combinations of kernal, C values, and gamma values. Non-linear kernel `RBF` initially gave better results when a subset of traing data were used. After using all training data, even linear svm classifier has test accuracy greater than 0.99 and `RBF` kernal classifier took much longer time for both training and testing, therefore I chosed a linear classifier as the final model. The code is in cell [fill number](??) of the jupyter notebook [T1P5 Pipeline submit.ipynb](??). The features include YCrCb HOG 3-channel features, spatially binned color and histograms of color. 

### Sliding Window Search

#### 1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?
The search approach was based upon extracting HOG featuers of the whole image scaled at different ratios. Here, I used 6 different scales ranging from 1.5 to 2 with 0.1 step interval. These set of scales produced a classifier with the best accuracy. 


#### 2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?
Here are examples of six test image's pipeline. I tuned different `bin_numbers`, `pixels_per_cell`, `cells_per_block` for feature exatraction and used grid search to pick parameters for SVM classifier. The pipeline basically includes steps of searching windows, generating heat mapping based upon detected windows, labeling pixels for different vehicles, and drawing bounding boxes around vehicles. 

![alt text][image2]
---

### Video Implementation

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
[![video image](https://img.youtube.com/vi/wl1l1Jf3NRY/0.jpg)](https://youtu.be/wl1l1Jf3NRY)


#### 2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

To detect vehicles, the number of windows overlaped for a particular pixel was counted. For each pixel, a threshold for overlapped window number was applied to filter false positive. I also applied two different threhold values depending on the maximum overlpaped winow number in a image with a higher threshold vlaue for maximum window number over 30. The code is in cell [fill number](??). In addition, two consecutive frames were averaged to serve as another way to reduce false positives. 


---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

There are several problems revealed after using the pipeline for project video processing. First, more than expected false positives were detected. It is hard to cleanly eliminate all false positives while keeping all true positives by using a fixed threshold value for heatmap. Second, bounding boxes can not be differentiated for two vehicles when they were mostly overlapped in the frame. For the first problem, by tracking the centroids of the heat map for filtering would help to build a more robust filter. For the second one, kalman filter could be exprimented to filter different vehicles labels. 

