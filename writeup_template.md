# **Finding Lane Lines on the Road** 

## Writeup

---

### Reflection

### 1. Description of my pipeline.

My pipeline consisted of 5 steps:

 1. get the top view perspective of the road and get the color mask for yellow and white.
 2. get the canny mask (find the edges) in normal view and than transform it to top view.
 3. broaden the found yellow and white area in color mask. Then combine it with canny mask and only keep the overlapping area, to filter out noises and retain useful information as much as possible.
 4. find the hough lines in top view and transform them back to normal view.
 5. get the thetas and rhos of the hough lines and average them to get only one line for each lane.
 6. draw the two lines on image.

During the perspective transform the region of insterest is selected. So the `cv2.fillpoly()` is not used here.

I didn't modify `draw_lines()` function. Instead I average the hough coordinates of the found hough lines to get one average hough coordinate for left lane and one for the right. Then I draw these two using `draw_lines()`.

I do the fusion of two masks mainly to filter out the noises in the optional challenge.

I do the perspective transform to filter out the occasional lateral line segments as noises, which is usually shorter. In top view I can set the `min_line_len` relatively high.

I do the perspective transform seperately for two masks, because if I do it after fusion, the upper part will get much more points than lower part due to the dense transform. And in the top view of the image I couldn't get reasonable canny edges.

At the end, I average the hough lines using length as weight.


### 2. Potential shortcomings with my current pipeline


One shortcoming could be that when the road keeps being shadowed, the color of white lane falls out of the range I setted for white color, then no white lane line will be detected.

Another shortcoming is curve lanes. In top view the line segments in far end of the curve lane are also well detected. So that after averaging the two lane lines are kind of indicating the drive direction after 2 seconds in the curve lane, instead of the drive direction at the moment.


### 3. Suggest possible improvements to my pipeline

A possible improvement would be to detect curve lines instead of straight lines using hough transform.

Another potential improvement could be to improve the averaging algorithm, to make it smarter, possibility based. So that some unreasonable hough lines will be filtered out and I can set the thresholds of the prior processes looser to get more useful information.
