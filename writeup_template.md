# **Finding Lane Lines on the Road** 


[//]: # (Image References)

[image1]: ./temp_images/solidYellowLeft_0.jpg "Figure 1"
[image2]: ./temp_images/solidYellowLeft_2.jpg "Figure 2"
[image3]: ./temp_images/solidYellowLeft_3.jpg "Figure 3"
[image5]: ./temp_images/solidYellowLeft_5.jpg "Figure 5"
[image4]: ./temp_images/solidYellowLeft_region_3.jpg "Figure 4"



### Reflection
The goal of this project is recognition of lanes on a road in test videos. Processing a lane lines video is nothing different as processing a single images. As a lane dividers on a road occures basiclly in two colors and they are built straight lines (from car camera perspective) I had to use a set of algorithm to identify these lines.


### 1. Pipeline to find a lane lines

My pipeline consisted I listed below. I shown the result of some step using an image _solidYellowLeft.jpg_ stored in test images folder.

* convert image in RGB color space to HSL color space. The HSL color space support yellow and white color in better way as the RGB color space.
* filter out all color beside of the mask of color created for yellow and white. The image below shows the result of conversion to HSL color space and filtering out color :

![alt text][image1]

* convert image to gray scale

![alt text][image2]

* apply a Gaussian Noise kernel
* apply the Canny transformation. See image below:

![alt text][image3]

* setup region of interest to limit an area of the image where further steps of pipline must be done. I defined a region of interest using constant coordinates. this is possoble as the car camera has constant position respectivlly to a road area. The region defined in this way built a  trapezoid. See image below :

![alt text][image4]

* apply Hough lines detection
* find average parameters of lines (slope and intercept) which match in best way to the lines defined in Hough space.
* draw lines found in this way on the input image. See image below :

![alt text][image5]

The left lane is represented in green color line and a right lane in red color.

#### 1a. Adaptation of _draw_lines_ function.
I used the **_draw_lines()_** function only as suggestion how to define my own pipeline to find lanes. The main steps, that are necessary to find line in Hough space were almost not changed. I used HSL color space instead of RGB color space. The HSL is better for yellow color separation.

The **_get_lane_lines()_** function implements finding of the line representation for left and right lane.
This is done in 2 main steps:
* calculation of line slope for lines provided as an output of Hough lines. To identify of a particular Hough line can represent a left or right lane I used a line slope.
The negative slope represents a left lane and a positive one a right lane. To avoid processing of lines which are almost paralell to X axis, I ignored a lines having abs(slope) < 0.3.
For every line I calulated it's length. I used this length as some weight to expose lines with a greather length.
* having an average line slope and intercept, I calculated a line end points for left and right lane separately.
I used Ymin value and Ymax a region of interest to define lane lines end point. See **_get_line_end_points()_** function.

The last step for drawing of lane lines on the original image is done in **_draw_lane_lines()_** function.


### 2. Identify potential shortcomings with your current pipeline

* One potential shortcoming would be what would happen when the camera in a car would be located at the different hight or having a different range of view angle. The constant values I used for the region of interest could be a wrong and could not match to the lanes visisble on road.
* Another shortcoming could be if the color of a lanes in real world would be different as the yellow and white color I used for my image processing pipline.
* The constants I used for Hough lines could be not optimal for other images used for processing.

### 3. Suggest possible improvements to your pipeline

* A possible improvement would be to find better way to filter a lines being almost parallel to X axis.
* Another potential improvement could be to find the Hough lines parameters using some statistics what kind of parameters get better result. Right now the parameters are more or less ‘try and look on result’.


