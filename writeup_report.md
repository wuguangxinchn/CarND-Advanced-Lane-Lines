# Advanced Lane Finding Project

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

[image1]: ./output_images/01-undistortion.png
[image2]: ./output_images/02-undistortion.png
[image3]: ./output_images/03-combine_threshold.png
[image4]: ./output_images/04-perspective_transform.png
[image5]: ./output_images/05-perspective_transform.png
[image6]: ./output_images/06-perspective_transform.png
[image7]: ./output_images/07-fitpoly.png
[image8]: ./output_images/08-image.png
[image9]: ./output_images/09-imagewithtext.png
[video1]: ./output_videos/project_video.mp4

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code below is to computed the camera matrix and distortion coefficients using the 20 images under folder camera_cal. The OpenCV function findChessboardCorners can help us find the chessboard corners. Please be noted these are 9x6 chessboard images, unlike the 8x6 images used in the lesson.  
I then used the output objpoints and imgpoints to compute the camera calibration and distortion coefficients using the cv2.calibrateCamera() function. I applied this distortion correction to the test image using the cv2.undistort() function and obtained this result.  
The camera calibration and distortion coefficients are stored using pickle to be used for other images.

![alt text][image1]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

![alt text][image2]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I have applied Sobel, Magnitude of the Gradient, Direction of the Gradient, and S-channel of HLS seperately, and then tried several combinations to get the best result. 

![alt text][image3]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

To perfrom the perspective transform more clearly, I will use a image with straight lines.  
Firstly I choosed 4 points along the two lanes. And then perform the perspective transform by cv2.getPerspectiveTransform function.

![alt text][image4]  
![alt text][image5]

After that I applied perspective transform to the binary images after combined threshold

![alt text][image6]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

General approach we used to identify lane-line pixels is "Sliding Window".  
The first step is to split the histogram into two sides, one for each lane line. Then set a few hyperparameters related to our sliding windows, and set them up to iterate across the binary activations in the image. After that I looped for nwindows, and finally fit a polynomial to the line.

![alt text][image7]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in calc_curv_rad_and_center_dist function 

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

The key processing in this step is to warp the lane area back to the perspective of the original image using the inverse perspective matrix Minv and overlaid onto the original image.

![alt text][image9]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's the [link to my video result](video1)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

I tried to apply the code to challenge_video and harder_challenge_video, but the result is not good. Will update the code in the furture to make it more robust. 
