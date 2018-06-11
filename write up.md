**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./results/camera.png "Undistorted"
[image2]: ./results/threshold.png "Road Transformed"
[image3]: ./results/warp.png "Binary Example"
[image4]: ./results/fit.png "Binary Example"
[image5]: ./results/radius.png "Fit Visual"
[image6]: ./examples/example_output.jpg "Output"
[video1]: ./project_result.mp4 "Video"

---


### 1. Camera Calibration


According to the images which are stored in the folder called `camera_cal`,the camera has radial distortion and tangential distortion.I would compute the camera calibration matrix with objects points and image points.Assuming the chessboard is fixed on the (x, y) plane at z=0,such that the object points are all the same for each calibration image,just the known object coordinates of the chessboard corners for an nine by five board.First,I used np.zeros and np.mgrid to generate `objp`, and then `objpoints` would be appended with a copy of it when i detect all chessboard corners in a test image.The `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.And then i used the opencv fucntion cv2.findChessboardCorners() to find chessboard corners and cv2.undistort() to undistort test images,one of the results as follows: 

![alt text][image1]

###  2.Pipeline

#### 1. Binary image result.

I used a combination of RGB color threshold and Lab color threshold to generate a binary image. I used RGB color threshold by tuning the parameter within the range of white color in RGB space to find the white lane line.I used Lab color threshold by tuning the parameter within the range of white color in Lab space to find the yellow lane line.Finally combined the white and yellow lane line to get the binary image result.The result of a test images as follows:

![alt text][image2]

#### 2. Perspective transform 

Now we need to perform the perspective transformation to birds eyes view images. I choose four source points and four destination points manually.Here are the points i choose:

```python
src = np.float32([[293, 668], [587, 458], [703, 458], [1028, 668]])
dst = np.float32([[310, im_shape[1]], [310, 0],
                          [950, 0], [950, im_shape[1]]])

```

After perspective transform,the warpped image look likes this:
![alt text][image3]



#### 3.Fitting a polynomial to the lane

 I first took a histogram along all the columns of the image,then implemented sliding windows method to detect lane 
line.Pixels are either 0 or 1, so the two most prominent peaks in the histogram will be good indicators of the x-
position of the base of the lane lines,and I used that as a starting point for where to search for the lines.Then fit a polynomial to those pixel positions.The result of the test image as follows:

![alt text][image4]

#### 4. Compute the radius of curvature 

We can use the mathematical formation and the parameters of polynomial to compute the radius of curvature of the fit.We can also get the positon of the car relative to the center of the lane line.The result of the test image as follows:

![alt text][image5]


---

### Pipeline (video)


Here's a [link to my video result](./project_result.mp4)

---

### Discussion

The pipeline is effective to project video but the result is not robust enough when performing at the challenge video.We can try use more computer vision knowledge and deep learning model to get more robust method.