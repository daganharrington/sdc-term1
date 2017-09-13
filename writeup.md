# **Finding Lane Lines on the Road** 

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps.
1-convert to grayscale
2-apply canny edge detection
3-isolate the region of interest
4-create image with lines from hough space, and black space elsewhere
5-overlay image from 4 onto original image

My goal with drawlines was to try to bin line segments based on their slope. The generality of my bins
was based on rounding the slopes to 1 decimal place (e.g. slope=3.14 and slope=3.19 go in the same 3.1 bin).
I made a rather large assumption by taking the top two bins with the most associated line segments as
the two bins I would consider the lane lines. This strategy is highly susceptible to noise in the environment/image.

I could draw a line for each bin relying on `max` and `min` being defined in python for 2-tuples as I need them to be.


### 2. Identify potential shortcomings with your current pipeline

As said, I think my binning assumptions in the draw_lines method are dangerous.
A good amount of 'binnable' noise could derail the generalization.

Another major shortcoming was my strategy of "twiddling" values for canny and hough
to come up with the best result on the videos and images. It would be more appopriate to try
and learn what parameters are best rather than flipping values until the result looks
decent to the human eye. There's no knowing whether these values generalize well to unseen video.


### 3. Suggest possible improvements to your pipeline

w.r.t. to the draw_lines method, it would be interesting to introduce some
hysteresis/memory into the system. For now, each image is processed in isolation.
The lane lines could be expected to remain somewhat stable across multiple frames...
so it would likely be helpful to know what the lane lines were assumed to be for previous
images in a stream.

Also, as mentioned above ... picking magic numbers as parameters to the helper functions
is not, in general, a good policy. Some way to learn these parameters or at least tune them
in a more analytic fashion would be great.