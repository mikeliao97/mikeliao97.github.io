---
title: 'Learning C++'
date: 2019-03-19 00:00:00
featured_image: '/images/demo/demo-portrait.jpg'
excerpt: Learning C++
---


Recently I've been wanting to learn c++

## 1. Including Libraries
The standard way of including inclues using double-quotes("") as well as brackets(<>)
{% highlight cpp %}
#include <iostream> // This means to look  where the standard libraries are stored

#include "main.hpp" // This means to look in the current directory, if the file is not there then look in the directory where the standard libraries are stored. 

{% endhighlight %}

## Difference between "\n" and endl;
{% highlight cpp %}
cout << x << endl;

is equivalent to 

cout << x << '\n';
cout.flush();
{% endhighlight %}

The flushing is different in some cases like a mulithreaded program.

## Type Aliases
A type alias is a different name by which a type can be identified. In C++ any valid type can be aliases so that it can be referred to a different identifier.
{% highlight cpp %}
typedef char C;
typedef unsigned int WORD;
typedef char * pChar;
typedef char field[50];

/* This defines 4 type aliases: C, WORD, pChar, and field as char. Once the aliases are defined, they can be used in any delcaration just as any other valid type */

C mychar, anotherchar, *ptcl;
WORD myword;
pChar ptc2;
field name;

/* A second syntax to define type alias recently */
using new_type_name = existing_type;
using C = char;
using WORD = unsigned int;
using pChar = char *;
using field = char[50];
{% endhighlight %}
**This creates a new union type, in which all its member elements occupy the same physical space in memory. The size of this type is the one of the largest member element. Modificiation of of the members will affect the value of all of them.**

## Unions
Unions allow one portion of memory to be accessed as different data types. Its declaration and use is similar to the one of structure, but its functionality is different. 

{% highlight cpp %}
union mytypes_t {
    char c;
    int i;
    float f;

} mytypes;

mytypes.c
mytypes.i
mytypes.f
{% endhighlight %}

## IOManip Library
The iomanip is in the std with #include <iomanip> and has functions like the following to manipulate output.
1. setiosflags
2. setbase
3. setfill
4. setprecision
5. setw
6. get_money
7. get_time

## 5. Fstream
To read and write files the general steps are
1. Include the <fstream> library
2. Create a stream (input, output, both)
    - ofstream myfile; for writing to a file
    - ifstream myfile; (for reading to a file)
    - fstream myfile; (for reading and writing)
3. Open the file myfile.open("filename");
4. Write/read the file
5. Close the file myfile.close();

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