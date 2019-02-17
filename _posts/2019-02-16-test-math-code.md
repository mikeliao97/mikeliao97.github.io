---
title: 'Math and Programming Blog'
date: 2018-06-30 00:00:00
featured_image: '/images/demo/demo-portrait.jpg'
excerpt: This page is a demo that tests out math and programming functionality 
---

## Math and Programming 

Example Python Program:
{% highlight python linenos %}
  def validMountArray(self, A):
    i, j, n  = 0, len(A) - 1, len(A)
    while i + 1 < n and A[i] < A[i+1]: i += 1
    while j > 0 and A[j - 1] > A[j] : j -= 1
    return 0 < i == j < n - 1
{% endhighlight %}


## Test Math
# Not rendered
$ x+1 $ 

## Inline
$$ x+1 $$ 
\\( x+1 \\) 

#### Block equation
\begin{equation}
x+1
\end{equation} 

#### The mean of a sum
$$mean = \frac{\displaystyle\sum_{i=1}^{n} x_{i}}{n}$$

## Try hot reloadings
$$ 2 + 4 $$
