---
title: 'OpenCV & Numpy'
date: 2019-03-21 00:00:00
featured_image: '/images/demo/demo-portrait.jpg'
excerpt:   OpenCV and Numpy
---


Currently I'm going through basic opencv and numpy function

## 1. Cool OpenCV Functions
{% highlight python %}
cv2.inRange() // for color selection
cv2.fillPoly() // fo region selection
cv2.line() /// to draw lines on an image given endpoints
cv2.addWeighted() // to coadd / overlay two images
cv2.cvtColor() // to grayscale or change color
cvw.imwrite() // to output images to file
cv2.bitwise_and() // to apply a mask to an image
{% endhighlight %}

1. Apply Gaussian filter to smooth the image in order to remove the noise
2. Find the intensity gradients of the image
3. Apply non-maximum suppression to get rid of spurious response to edge detection
4. Apply double threshold to determine potential edge
5. Track edge by hysteresis: Finalize the detection of edges by suppressing all the other edges that are weak and not connected to strong edges. 

There are two tunable parameters in OpenCv's implementation: Kernel Size, and low/high threshold. 

1. Low/High Threshold
A way to automatically find good values is to take the median of the pixel values and take values that are a standard deviation above/below as high/low. The code looks something like this:
{% highlight python %}
v = np.median(image)
std = 0.33
low_thresh = int(max(0, (1.0 - std) * v))
high_thresh = int(min(255, (1.0 + std) * v))
{% endhighlight %}

2. Picking Kernel Size 
As kernel size increases, more pixels are part of the convolution process. Thus, the gradient map will get more blurry. 


## 2. Hough Line Transform Parameters
1. **dst**: Output of the edge dector. It should be a grayscale image (although in fact is is a binary one)
2. **lines**: A vector that stores the parameter ($$r$$, $$\theta$$) of the detected lines
3. **rho**: The resolution of the parameter r in pixels. We use 1 pixel.
4. **theta**: The resolution of the parameter $$\theta$$ in radians. We use 1 degree (CV_PI/180)
5. **threshold**: The minimum number of intersections to "detect" a line
6. **srn** and **stn**: Default parameters to zero. Check OpenCV reference for more info