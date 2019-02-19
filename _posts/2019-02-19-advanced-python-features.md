---
title: 'Advanced Python Features'
date: 2019-02-19 00:00:00
featured_image: '/images/demo/demo-portrait.jpg'
excerpt: Advanced Python Features 
---

Recently, I'm been using Python alot, so I wanted to explore the advanced python features. Here are some things about Python that I learned. 
I read a bunch of blog posts as well as Fluent Python. Here are my summarized findings. 

## 1. Packing and Unpacking 
If you are your arguments in a list or a dictionary, you can the unpacking operators * and ** to pass the arguments.
{% highlight python linenos %}
  def repeat(count, name):
      for i in range(count):
          print(name)

  print("Call function repeat using a list of arguments:")
  args = [4, "cats"]
  repeat(*args)

  print("Call function repeat using a dictionary of keyword arguments:")
  kwargs = {'count': 4, 'name': 'cats'}
  repeat(**kwargs)
{% endhighlight %}


## 2. Decorators
A decorator is a function which takes a function as a parameter and returns a function. A practical example of a decorator is a cache to for certain inputs. 
{% highlight python lineos %}
  def cache(function):
      cached_values = {}  # Contains already computed values
      def wrapping_function(*args):
          if args not in cached_values:
              # Call the function only if we haven't already done it for those parameters
              cached_values[args] = function(*args)
          return cached_values[args]
      return wrapping_function

  @cache
  def fibonacci(n):
      print('calling fibonacci(%d)' % n)
      if n < 2:
          return n
      return fibonacci(n-1) + fibonacci(n-2)

  print([fibonacci(n) for n in range(1, 9)])
{% endhighlight %}

## 3. Functools
**Functools** provides a few decorators such as a **lru_cache** 
{% highlight python lineos %}
  from functools import lru_cache

  @lru_cache(maxsize=None)
  def fibonacci(n):
      print('calling fibonacci(%d)' % n)
      if n < 2:
          return n
      return fibonacci(n-1) + fibonacci(n-2)

  print([fibonacci(n) for n in range(1, 9)])
{% endhighlight %}

## 4. Searching with Bisect and Insort
The **bisect** module offers two main functions—**bisect** and **insort**—that use the binary search algorithm to quickly find and insert items in any sorted sequence.”
A good example in the book. For example, if you had a list of scores and wanted to assign letter grades, how would you do it? A good way is to define a list of ranges and use binary sort to check if a score is within that set. 
{% highlight python lineos %}
>>> def grade(score, breakpoints=[60, 70, 80, 90], grades='FDCBA'):
...     i = bisect.bisect(breakpoints, score)
...     return grades[i]
...
>>> [grade(score) for score in [33, 99, 77, 70, 89, 90, 100]]
['F', 'A', 'C', 'C', 'B', 'A', 'A']

Excerpt From: Luciano Ramalho. Fluent Python. Apple Books. 
{% endhighlight %}

Sorting is expensive, so the **insort** module is used to insert a element in sorted order. 

## 5. Use arrays instead of lists when the list is large 
For very large data like a list of 10 million floats, an **array** might be a better choice. The difference between a **list** and an **array** is that **list** will hold 10 million **float objects** while arrays will only hold values like in a C array(and not the whole object).
{% highlight python lineos %}
>>> from array import array  
>>> from random import random
>>> floats = array('d', (random() for i in range(10**7))) ”
>>> floats[-1]  
0.07802343889111107
>>> fp = open('floats.bin', 'wb')
>>> floats.tofile(fp)  
>>> fp.close()
>>> floats2 = array('d')  
>>> fp = open('floats.bin', 'rb')
>>> floats2.fromfile(fp, 10**7)  
>>> fp.close()
>>> floats2[-1]  
0.07802343889111107
>>> floats2 == floats  
True”
Excerpt From: Luciano Ramalho. “Fluent Python.” Apple Books. 
{% endhighlight %}
This is an example of creating, saving, and loading a large number of floats. In this case, loading with **array.fromfile** is ~60x faster than using **open**. Saving with **array.tofile** is ~7x faster than traditionally writing one number to a file at a time. 

## 6. Dicts
Dictionary comprehension is the same thing as list comprehension
{% highlight python lineos %}
>>> items = [(1, 'one'), (2, 'two'), (3, 'three')]
>>> dict_items = {key:val for key, val in items}
{1: 'one', 2: 'two', 3: 'three'}
{% endhighlight %}