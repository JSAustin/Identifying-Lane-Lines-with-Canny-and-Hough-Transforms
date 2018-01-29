# **Finding Lane Lines on the Road** 

## Writeup

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps:
1) Convert color image to greyscale image.
2) Apply Gaussian blurring using a kernel size of 5.
3) Detect edges using a canny detection algorithm with OpenCV (low threshold: 70, high threshold: 190)
4) Apply region selection mask to exclude edges detected outside of the region of interest (lane lines and road in front of car).
5) Detect lines from edge points using Hough Transform.
  a) draw_lines() was modified.  In the new algorithm, lines are divided into either left or right lane lines by whether the line's slope is positive or negative.  Lines are excluded if the magnitude of the slope is either too small (< 0.4) or too large (> 1.2).  The average slope and intercept is then taken separately for the left and right lines and used to plot new lines that originate at the bottom of the image and terminate at the highest acceptable edge detected in that frame.
6) Overlay lines onto original color image.

### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when the car is heading up/down steep hills or is making sharp corners.  The region mask would not capture the correct road segments.

In the case of sharp corners, the slope may not fit within the selected range.

Another shortcoming could be on sharp corneers when a single straight line originating at the car fails to overlap the lane lines because there is too much curvature.

Another shortcoming would be if there is road construction and there is a partial shift in lanes, so the driver is driving over an old lane line they should ignore.

Another shortcoming would be if there is a visible line in the pavement that isn't a lane line but has a sharp contrast, such as when one lane is made with concrete and the other is made with pavement.

Another shortcoming would be if a car exists directly in the same lane in front of the car, obstructing the real lane lines.  The edges on the car might be confusing and might give incorrect lane lines.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to allow curved lines to accomidate sharp turns.

Another potential improvement could be to code in that lane lines must be either yellow or white.

Additionally, you could ignore a triangular region in the center of the lane in front of the car where you don't expect to see any lane lines.
