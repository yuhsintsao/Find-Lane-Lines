# **Finding Lane Lines on the Road** 

## Writeup 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on my work in a written report

[image1]: ./test_images_output/solidWhiteCurve_blur.jpg "Gaussian blur"

[image2]: ./test_images_output/solidWhiteCurve_RG.jpg "Drop blue channel"

[image3]: ./etest_images_output/solidWhiteCurve_ROI.jpg "ROI"

[image4]: ./test_images_output/solidWhiteCurve_edge.jpg "Edge detection"

[image5]: ./test_images_output/solidWhiteCurve_hough.jpg "Hough transformation"

[image6]: ./test_images_output/solidWhiteCurve_out.jpg "Image with road lines detected"

---

### Reflection

### Pipeline description.

My pipeline consisted of 6 steps:

(1) Apply Gaussian blue function to the image in order to reducing the noise. The kernel size can be set as 3 or 5. A smaller kernel size is sufficient to reduce the noise. Too larger size makes it hard for edge detection.

![Gaussian Blur][image1]

(2) Set the blue color channel to be 0. The road lines are more commom to be drawn in yellow and white. To make the edge detection more efficient, I drop the blue channel by setting all numbers as 0.

![RG Channel][image2]

(3) Select a trapezoid region with its 4 points set as (0, 540), (548, 265), (548, 275), (960, 540). Selecting a ROI region avoids false detection of road lane linse.

![ROI][image3]

(4) Apply Canny edge detection to extract edge lines in the image. I set the low and the high threshold as 50 and 150 respectively.

![Edge Detection][image4]

(5) Use hough transformation to make edge lines smooth and continous. The shape of road lines is rectangle with a larger length to width ratio. Thus, a continuous linear line in the image is more likely to be a road lane line.

![Hough Transformation][image5]

(6) In order to draw a single line on the left and right lanes, I modified the draw_lines() function by picking the maximun and minimum x and y values forming all lines from the output of the previous step. The parameters are set as following: (rho = 2, theta = pi/180, threshold = 10, minimum line length = 50, max line gap = 30). 

![Final Image][image6]

---

### 2. Shortcomings

One potential shortcoming would be the false detection if there are too many white or yellow cars on the road. Cars are the objects which are more likely to be detected in the ROI region. If the minimin line length is set to be too small, the edge of a car with similar color as the lane line's would be counted. The miscounting would result in extreme values of (x,y) which are used as end points of the left and right line drawn in the last step (step 6). 

Another shortcoming could be the failure of distinguishing the edge of road side from a real lane line. Sometimes the color of road surface is close to white. Compared to the sidewalk made of dark-colored concrete, edge detection may fail due to the weaker edge of lane lines.

---

### 3. Future Improvements

A possible improvement would be to calculate regression lines for the left and the right lane line separately. This gives me more accurate information of the slope of real lane lines. In that case, if the draw_lines function returns a line with slope very different from the previous one, I may drop a few points with extreme x or y values and plot another line. 

Another potential improvement could be to make adjustment to the lines based on the information from the adjacent video clips. Because video clips extrated from a very short time frame should share similar information of the position of lane lines. Therefore, analyzing x-1, x, x+1 video clips simultaneously for each line detecting operation (or for each loop) may be helpful to avoid failure due to the presense of objects, e.g. cars.
