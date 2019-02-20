---
title: 'Advanced Python Features'
date: 2019-02-19 00:00:00
featured_image: '/images/demo/demo-portrait.jpg'
excerpt: Advanced Python Features 
---

Recently, I'm been using Python alot, so I wanted to explore the advanced python features. Here are some things about Python that I learned. 
I read a bunch of blog posts as well as Fluent Python. Here are my summarized findings. 

## 1. Packing and Unpacking 
If you are your arguments in a list or a dictionary, you can the unpacking operators * and ** to pass the arguments. This turns out to be very useful because you can create very flexible functions. Below is an example of a function outputting html based on a variety of inputs.
{% highlight python %}
def tag(name, *content, cls=None, **attrs):
      if cls is not None:
          attrs['class'] = cls
      if attrs:
          attr_str = ''.join(' %s="%s"' % (attr, value)
                            for attr, value
                            in sorted(attrs.items()))
      else:
          attr_str = ''
      
      if content:
          return '\n'.join('<%s%s>%s</%s>' %
                          (name, attr_str, c, name) for c in content)
      else:
          return '<%s%s />' % (name, attr_str)

>>> tag('br')   
'<br />'
>>> tag('p', 'hello')   
'<p>hello</p>'
>>> print(tag('p', 'hello', 'world'))
<p>hello</p>
<p>world</p>
>>> tag('p', 'hello', id=33)   
<p id="33">hello</p>
>>> print(tag('p', 'hello', 'world', cls='sidebar'))   
<p class="sidebar">hello</p>
<p class="sidebar">world</p>
>>> tag(content='testing', name="img")
<img content="testing" />
>>> my_tag = {'name': 'img', 'title': 'Sunset Boulevard',
              'src': 'sunset.jpg', 'cls': 'framed'}
>>> tag(**my_tag)
<img class="framed" src="sunset.jpg" title="Sunset Boulevard" />

Excerpt From: Luciano Ramalho. Fluent Python. Apple Books. 
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
Python contains many implementation of dict, but need to import *collections*:
1. dict (regular)
2. DefaultDict (default value)
3. OrdereDict (ordered keys)
4. ChainMap (many dicts that are searched in order)
5. MappingProxyType (can be used to create immutable map)

Personally, I find the MappingProxyType very interesting. **To create secure code and to share data across teams, you might want to create a dictionary that can't be modified.** This is an example of hwo to do so. 
{% highlight python lineos %}
>>> from types import MappingProxyType
>>> d = {1: 'A'}
>>> d_proxy = MappingProxyType(d)
>>> d_proxy
mappingproxy({1: 'A'})
>>> d_proxy[1]  
'A'
>>> d_proxy[2] = 'x'  
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'mappingproxy' object does not support item assignment
>>> d[2] = 'B'
>>> d_proxy  
mappingproxy({1: 'A', 2: 'B'})
>>> d_proxy[2]
'B'
>>>
Excerpt From: Luciano Ramalho. Fluent Python. Apple Books. 
{% endhighlight %}


Dictionary comprehension is the same thing as list comprehension
{% highlight python lineos %}
>>> items = [(1, 'one'), (2, 'two'), (3, 'three')]
>>> dict_items = {key:val for key, val in items}
{1: 'one', 2: 'two', 3: 'three'}
{% endhighlight %}

## 7. Sets
*Sets* and *Frozensets* are implemented with hash tables under the hood. 

### Why can't we add to sets/dicts while iterating through?
When you add to sets/dict while iterating, the Python interpreter might
decide that the underlying hash table needs to grow. This entails creating a new hash table where the order of the keys are different. Thus, when you iterate, you might not see all the elements. 

## 8. Texts vs Bytes
One thing I was not aware of was how many different types of encoding there were for python. Most python programmers use ASCII, or utf-8. However, there are many types of encoding such as:
#### 1. gb2312
Used to encode the simplified Chinese ideographs used in mainland China; one of several widely deployed multibyte encodings for Asian languages.
#### 2. cp1252
A latin1 superset by Microsoft, adding useful symbols like curly quotes and the € (euro); some Windows apps call it “ANSI,” but it was never a real ANSI standard.

There are many more encodings for different countries (Russia, South America) and different types of computers (Windows, IBM), but mostly I don't care enough so I skipped it. 


## 8. Function Annotations
Python is not a statically typed language, but Python3 introduces Function Annotations which provide "hints" for what the function is suppose to do. These "hints" do not raise errors, so it is still up to the user to validate the inputs however :(

{% highlight python %}
def foo(bar:str, baz:'int>0') -> str:
  new_str = bar + str(baz)
  return new_str

>>> foo.__annotations__
{'bar': str, 'baz': 'int >0', 'return': str}
>>> foo("two", 2)
'two2'
{% endhighlight %}



## Modern Pythonistic Way for map, reduce, filter