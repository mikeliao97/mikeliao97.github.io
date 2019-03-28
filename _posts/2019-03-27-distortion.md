---
title: 'Distortion'
date: 2019-03-27 00:00:00
featured_image: '/images/demo/demo-portrait.jpg'
excerpt:  Computer Vision Distortion
---

I would like to get better at using Python Decorators. 

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
