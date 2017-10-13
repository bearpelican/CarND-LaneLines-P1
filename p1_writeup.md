# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./image_transformations/gaussian.jpg "Gaussian"
[image2]: ./image_transformations/grayscale.jpg "Grayscale"
[image3]: ./image_transformations/canny.jpg "Canny"
[image4]: ./image_transformations/mask.jpg "Mask"
[image5]: ./image_transformations/region.jpg "Region"
[image6]: ./image_transformations/mask_region.jpg "Mask Region"
[image7]: ./image_transformations/lines_hough.jpg "Hough"
[image8]: ./image_transformations/lines_avg.jpg "Average Lanes"
[image9]: ./image_transformations/lines_ext.jpg "Extend Lanes"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

Pipeline consisted of 7 steps of transformations

Step 1: Apply gaussian. Filter of 9px worked best for me
![Gaussian][image1]

Step 2: Convert to grayscale
![Grayscale][image2]

Step 3: Canny filter - set the repective min and max thresholds to 90px and 180px
![Canny][image3]

Step 4: Mask - focusing on only the regions we care about.
    I hardecoded the vertices if the image size was: 960 x 540
    Otherwise, I used a percentage
![Mask Region][image5]

Here's what we detected
![Region][image4]

Step 5: Hough transform - 
Values used:
```python
    rho = 1
    theta = np.pi / 180
    threshold = 40
    min_line_len = 50  # Higher value to detect lane lines
    max_line_gap = 200  # High value for dotted lines
```
![Hough][image7]

Step 6: Average all the hough lines to produce a single left and right lane
![Average][image8]

Step 7: Extrapolate and extend both ends the averaged lines. 
This is needed to connect the dotted lines to the image edges
![Average][image9]


### 2. Identify potential shortcomings with your current pipeline

This is a very shallow implementation of lane detection

Problem 1: Not very good at separating the left and right lanes
I am filtering the left and right lanes by looking at the sign of the slope - y=mx+b
This means we always expect the left lane to have a postive slope and right to have a negative slope
Any curves in the road will cause the lane detection to mess up

Problem 2: Only works on straight highways
The draw lanes function only works on lines. Currently not able to draw curved lines to follow a curved road

Problem 3: Can only detect lane in front
I am using a very small mask region and ingnoring other regions.

Problem 4: Image color and brightness
The canny filter thresholds are hardcoded. This means a bright sunlit image or very dark image will break this detection




### 3. Suggest possible improvements to your pipeline

Addressing Problem 1:
Use kmeans or some other clustering algorithm to separate left lane, right lane, and noise

Addressing Problem 2:
Instead of averaging the lane lines, fit a quadratic function and plot it

Addressing Problem 3:
Once lane detection is better, increase the mask size
Or you can use different regions of the images to detect different lanes

Addressing Problem 4:
Instead of grayscaling - specifically look for yellow or white color
Remove hardcoded canny filter thresholds. Instead, base the thresholds off a standard deviation of all the pixel colors in the region