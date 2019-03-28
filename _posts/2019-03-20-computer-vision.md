---
title: 'Computer Vision'
date: 2019-03-19 00:00:00
featured_image: '/images/demo/demo-portrait.jpg'
excerpt:  Learning Computer Vision
---


Currently I'm going through basic computer vision techniques and here are somethings I learned

## 1. Picking High and Low Threshold for Canny Edge Detection 
The Canny Edge detection algorithm can be broken down to 5 steps
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
