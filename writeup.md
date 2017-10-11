# **Finding Lane Lines on the Road** 

## Writeup

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. 
Before running the pipeline, I'd clear a memory of sorts of previous lane lines and their slopes.

1. Then, to begin with, I would read in the relevant image images to grayscale and use the GaussianBlur() function from the cv2 library with a kernel size of three to counter noise in the image data.

2. Next, I applied a Canny transform, also using a function (Canny()) from the cv2 library. Through this step, I selected all possibly relevant edges, which, in this case, are significant gradients, with a low threshold of 50 and a high threshold of 150. The result of this is illustrated below.

![Canny transformation result][canny_result.png]

3. Then I selected a quadrilateral region of interest, defined based on the image dimensions. This mask, if you will, was used to determine which of the previously found gradients (i.e., in step (2)) are relevant. The result of this is illustrated below.

![Masking result][masked.png]

4. Next, I drew in the Hough lines based loosely on the gradients. For this, I also used a function in the cv2 library; namely, HoughLinesP(), with a rho parameter of 1, a theta parameter of pi/180, a threshold of 25 to account for the fact that lane markings may fade and leave less "evidence" of lane markings, a minimum line length of 25 to account for the fact that lane markings may fade and thus shorten, and a maximum line gap of 200 to account for the fact that lane markings may fade and thus leave greater space between lane markings. The result of this is illustrated below.

![Hough lines][hough_lines.png]

5. Finally, I weighted the original, full-color image with the Hough lines drawn in step (4). To accomplish this, I used the addWeighted() function in the cv2 library and selected an alpha parameter of 0.8, a beta parameter of 1.0, and a lambda parameter of 0.0. The result of this is illustrated below.

![Final image with lane markings][weighted_image.png]

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by extrapolating lane lines from average slopes and positions detected and adjusting said extrapolations with respect to saved values in order to introduce a degree of consistency.

Specifically, I calculated the slope of each detected line segment and classified the line segment as belonging to the left or right lane line based on its positivity. Line segments with unreasonable slopes may be useless noise and were ignored.

The slope of each lane (i.e., left and right) was calculated as the mean of the slopes of the corresponding line segments whereas the fundamental lane position was calculated as the mean of the corresponding line segment coordinates. Both values were adjusted based on previously recorded values to introduce consistency between frames (the deduced lane markings were quite jittery before this feature was introduced).

After each calculation, the calculated values were saved in global variables. Before the processing of each video clip, these values would be cleared so as to address interference.


If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
