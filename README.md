# Project: Finding Lane Lines on the road [![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)


<img src="examples/laneLines_thirdPass.jpg" width="480" alt="Combined Image" />

# Project Description

When we drive, we use our eyes to decide where to go.  The lines on the road that show us where the lanes are act as our constant reference for where to steer the vehicle.  Naturally, one of the first things we would like to do in developing a self-driving car is to automatically detect lane lines using an algorithm.
In this project I will detect lane lines in images using Python and OpenCV.
The provided dataset consists of road recordings where the road markings are visible, and the goal is to outline the lane lines of the visible road. I built the image processing pipeline myself and applied it to the given test videos.

The requirements for this project can be found in the [CarND Term1 Starter Kit](https://github.com/udacity/CarND-Term1-Starter-Kit/blob/master/README.md).


# Project Pathway

In this project, I implemented a pipeline to detect the lane lines surrounding a car on the highway. To understand the pipeline I implemented, follow the notebook "Lane Lines Detection.ipynb". 
To extract meaningful information from the give images, I first tested the different transformations on test images.

Let us see what the road looks like:

<img src="readme_images\basic photo.png" width="549" alt="Combined Image" />

The first step was to discard any other color than white and yellow. After doing so, I obtained the following result:

<img src="readme_images\yellow_white_filter.png" width="549" alt="Combined Image" />

Then, after applying smoothing out using a Gaussian Filter and applying a Canny transform, I obtained the following:

<img src="readme_images\Gaussian and Canny edges detection.png" width="549" alt="Combined Image" />

Which results in the following once only the region of interest is kept

<img src="readme_images\carving region of interest.png" width="549" alt="Combined Image" />

Finally, after using the Hough Transform on the edges to outline the lines, I obtained the following picture:

<img src="readme_images\result example.png" width="549" alt="Combined Image" />



After testing these techniques on the provided test images, I regrouped all the above into a main pipeline, that I applied to a couple of videos. Here's a look at the rendering:

<img src="readme_images\yellow_left.gif" width="549"/> <img src="readme_images\white_right.gif" width="549"/>


# Conclusion

In this project, I was able to build a pipeline that recovers the lane lines surrounding the car using the provided videos. On simple scenarios, the pipeline is quite robust: indeed, I implemented three different methods to interpolate the lane lines, which allows for an increased robustness. One method uses a simple linear regression solver, the second uses a smart averaging weighted by the norms of the constituting lines and the last one uses the slope and interception point of the line with maximum norm. However, there are some improvement points:
1. First, the performance begins to drop when lighting conditions are changing a lot (lots of sunlight or shades), which is one point of improvement. This comes from the thresholding part, when we keep only white and yellow colors using specific thresholds. One way to fix that issue would be to compute thresholds specific to the image, that use a measurement of the global lighting of the interest zone to determine how to shift the threshold so as to still keep the colors of interest. In a case of low lightness, one would look to shift the thresholds so as to allow for colors with a lower "lightness parameter" (in the Hue, Saturation, Lightness format). I think this would be a nice improvement.
2. A second improvement would be to build to smooth the line interpolations over time: building a buffer to retain the previously computed slopes and interception points and average those over time, so as to avoid brutal change of slopes that do not happen in reality.
3. Finally, for the cases of lines with a strong curvature, it would be interesting to change the model of interpolation to the one of a higher order polynomial (for now I am interpolating a line: y = ax + b), which would change the Hough Transform step (since we are not looking for lines anymore). The process would be to first identify the edges corresponding to the left and right lane lines, and then fit a polynomial to these edges, to then draw it on the image.

# References

This project is provided within Udacity Self driving Car Engineer Nanodegree program. All the details are available on the [Udacity Github Repository](https://github.com/udacity/CarND-LaneLines-P1), and on the [Udacity website](https://www.udacity.com/course/self-driving-car-engineer-nanodegree--nd0013) related to the course. 
