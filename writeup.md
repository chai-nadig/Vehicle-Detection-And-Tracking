# Vehicel Detection and Tracking

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

## 1. Histogram of Oriented Gradients (HOG)

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

## 2. HOG parameters.

* HSV color space gave the best gradients. It performed better than other color space in frames with varied lighting conditions.
* 12 orientations were picked to enrich the features.
* 8 pixels per cell was chosen to achieve detection of gradients at a granular level.
* 2 cells per block allowed scanned the image thorougly.

## 3. SVM Classifier

1. The code for the SVM Classifier is in the 11th code cell.
2. The feature used to train the SVM is a long concatenated feature consisting of Spatial, Color and HOG features.
3. The dataset is randomly split into training and testing sets.
4. A `StandardScaler` is used to normalize the pixel values. The scaler is fit to the traning data and is used to transform both, the traning and testing data.
5. I achived a test accuracy of 99.13%

## 4. Sliding Window Search

1. Code cell 13 has the code that does the sliding window search
2. I decided to go with a scale of 1.5 because anything lesser would give too many boxes to search. Anything more would create boxes that are large and would miss smaller cars.
3. I decided to go with an overlap of 75% to cover as much of the image as possible. I wanted to scan the image thorougly.

## 5. Images
1. The images above depict the working of the pipeline.

## 6. SVC Optimization
1. No extra optimization was done for the SVC.
2. I ensured that the features provided to the SVC are rich. 
3. I also ensured that a randomized split of training and testing was used.

## 7. Video Implementation

#### 1. Here's a [link to my video result](./output_videos/project_video.mp4)

#### 2. Heat maps
* Code cell 12 contains the code to generate heat maps to eliminate false positives and draw bounding boxes around the cars.
* All bounding boxes detected by the classifer in each frame were recorded.
* A heat map was created for all the pixels in each of these bounding boxes. 
* `scipy.ndimage.measurements.label()` is used to identify individual blobs in the heatmap.
* Assuming each blob corresponded to a vehicle a bounding box to cover the area of each blob is drawn.

---

## 8. Discussion

#### 1. Problems

* The heatmaps now are not kept track between frames.
* If I kept track of the heatmaps from one frame to another, I will be able to detect the car pixels better.
* Right now the bounding box for the cars sometimes don't fully cover the car.
* Experiment with less than three channels. I've used three color channels when extracting color features. I would like to experiment with single or two color channels and see how it performs.
