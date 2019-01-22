PIPELINE: 

My pipeline consisted of following steps
1) Copy the original image.
2) Conver the image to HLS colorspace using opencv inbuilt function cv2.cvtCOLOR.
3) Now create white and yellow masks for the HLS image. 
4) bitwise AND the mask with the HLS image. 
5) The resultant image is then converted to grayscale.
6) Apply Gaussian blur to this grayscale image.
7) Find edges in the image using the Canny edge detection function. 
8) Define a region mask of a quadrilateral shape to fit our region of interest enclosing both the lines. 
9) Extract feature only within the mask and ignore the rest. 
10) Use Hough transformation to identify lines within the image and average and extrapolate the lane lines. 
11) Added the Hough transform image to the original image using weighted_image function. 

Modifications to the draw_lines function 
a) Segregate lines from Hough transform into "right" and "left" based ont heir slope. 
b) Collect points which belong to the left line and those that belong to the right lane. 
c) Of these points for left side and right side, find "n" number of points which have a minimum 'y' coordinate value and maximum 'y' coordinate value (top and bottom of the region of interest) and also their corresponding x coordinate values. Here 'n' is the desired number of points (n>2 would yield better results)
d) Find the average coordinates (mean of the x and y coordinates) of the left and right line maximum(top) and minimum(bottom) points. 
d) Using scipy's inbulit linear optimization function for finding the best fit, find slope of the line that best fits the points on the right and left.
e) Use the average coordinates of the maximum and minimum and the slope from linear regression to draw lines on the image. 
f) Now use the top and bottom region boundary values of the y coordinate to find the corresponding x coordinate using the best-fit slope. 
g) With the new x and y coordinates of the lines draw lines from top to bottom of the masked region. 


2. Potential shortcomings with current pipeline

1) Does not work well if the position of the camera changes with respect to the car since region of interest is hardcoded to be symmetric about center. 
2) Does not find curved lane lines. 
3) The averaged lines seem to osciallte from frame to frame. 
4) Large osciallations of the averaged lines are observed if extra lines are present within the region of interest other than the lane lines(for example, the short horizontal lines in the SoldiYellowLeft video) or if the lane lines are short. 


3. Possible improvements to the pipeline

1) A possible improvement would be to fine tune the Hough transformation paramters to identify lines with higher accuracy. 

2) Another improvement could be to fit splines to the identified edges instead of lines for 
detection of lane lines on curvy roads. 

Challenge video:
1) The challenge video seems to give a better result with a different set of Hough transform parameters (threshold of 5, min_line_length of 5 and max_line_gap of 2) with a smaller region of interest height-wise. 