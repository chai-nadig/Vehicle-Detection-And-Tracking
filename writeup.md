## Vehicel Detection and Tracking

---

**Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier.
* Apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Use normalized features and randomize a selection for training and testing.
* Implement a sliding-window technique and use the trained classifier to search for vehicles in images.
* Run the pipeline on a video stream and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[test1]: ./test_images/test1.jpg
[test1_out]: ./output_images/test1_out.jpg
[test1_hog_0]: ./output_images/test1_hog_channel_0.jpg
[test1_hog_1]: ./output_images/test1_hog_channel_1.jpg
[test1_hog_2]: ./output_images/test1_hog_channel_2.jpg

### Histogram of Oriented Gradients (HOG)

1. The code for this step is contained in the third code cell under the `get_hog_features` function. 
2. These are the hyperparameters I worked with for the final pipeline that was executed on the videos.

| **Hyperparamter**  |  **Remarks**                                    | **Value** |
|--------------------|-------------------------------------------------|-----------|
| `color_space`      | Channels of the image that were used to get HOG | **HSV**   |
| `orient`           | Number of orientation bins                      | **12**    | 
| `pix_per_cell`     | Number of pixels per cell                       | **8**     |
| `cell_per_block`   | Number of cells per block                       | **2**     |

3. Here is the original image with the car detected in it.

|**test1.jpg**       | **test1_out.jpg**          |
|--------------------|----------------------------|
|![test1.jpg][test1] | ![test1_out.jpg][test1_out] |

4. These image visualize the hog features in the H,S and V channels of the image

|**test1_hog_channel_0.jpg** | **test1_hog_channel_1.jpg** | **test1_hog_channel_2.jpg** |
|----------------------------|-----------------------------|-----------------------------|
|![test1_hog_channel_0.jpg][test1_hog_0] |![test1_hog_channel_1.jpg][test1_hog_1] | ![test1_hog_channel_2.jpg][test1_hog_2] |

#### 2. HOG parameters.

* HSV color space gave the best gradients. It performed better than other color space in frames with varied lighting conditions.
* 12 orientations were picked to enrich the features.
* 8 pixels per cell was chosen to achieve detection of gradients at a granular level.
* 2 cells per block allowed scanned the image thorougly.

#### 3. SVM Classifier

1. The code for the SVM Classifier is in the 11th code cell.
2. The feature used to train the SVM is a long concatenated feature consisting of Spatial, Color and HOG features.
3. The dataset is randomly split into training and testing sets.
4. A `StandardScaler` is used to normalize the pixel values. The scaler is fit to the traning data and is used to transform both, the traning and testing data.
5. I achived a test accuracy of 99.13%

### Sliding Window Search

#### 1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

I decided to search random window positions at random scales all over the image and came up with this (ok just kidding I didn't actually ;):

![alt text][image3]

#### 2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

Ultimately I searched on two scales using YCrCb 3-channel HOG features plus spatially binned color and histograms of color in the feature vector, which provided a nice result.  Here are some example images:

![alt text][image4]
---

### Video Implementation

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](./project_video.mp4)


#### 2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

I recorded the positions of positive detections in each frame of the video.  From the positive detections I created a heatmap and then thresholded that map to identify vehicle positions.  I then used `scipy.ndimage.measurements.label()` to identify individual blobs in the heatmap.  I then assumed each blob corresponded to a vehicle.  I constructed bounding boxes to cover the area of each blob detected.  

Here's an example result showing the heatmap from a series of frames of video, the result of `scipy.ndimage.measurements.label()` and the bounding boxes then overlaid on the last frame of video:

### Here are six frames and their corresponding heatmaps:

![alt text][image5]

### Here is the output of `scipy.ndimage.measurements.label()` on the integrated heatmap from all six frames:
![alt text][image6]

### Here the resulting bounding boxes are drawn onto the last frame in the series:
![alt text][image7]



---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  

