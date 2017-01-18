---
tags:
  - data-science
  - data-science-notes
  - python
---
Data Science Notes Series: Python Files and Libraries

{% include toc %}

# Working with external files

We'll be working with lots of external files in data science - that's where most of our data is going to me. So we'll need to learn some ways to load files into Python.

## open


```python
f = open("test.txt", mode="r", newline="\n")
```


```python
# .readlines() reads the whole file into a list, with each line as one element
f.readlines()
```




    ['Hello World!\n',
     'This is a random file\n',
     'With random lines of text\n',
     'For reading into Python\n']




```python
# With the open method, you have to close the file after you're done with it
f.close()
```


```python
# There is another method .readline() that will read it line by line
f = open("test.txt", mode="r", newline="\n")
```


```python
f.readline()
```




    'Hello World!\n'




```python
f.readline()
```




    'This is a random file\n'




```python
f.readline()
```




    'With random lines of text\n'




```python
f.readline()
```




    'For reading into Python\n'




```python
# If we continue to use .readline() after the last line in the file, we'll get an empty string
f.readline()
```




    ''




```python
# And remember to close the file!
f.close()
```

## with... as

`open` is useful, but you have to remember to close the file when you're done with it. Using the `with... as` syntax, we can do away with that requirement.


```python
with open("test.txt", mode="r") as f:
    print (f.readlines())
```

    ['Hello World!\n', 'This is a random file\n', 'With random lines of text\n', 'For reading into Python\n']


Using this, anything you want to do with the file has to be indented within the `with` block of code. You could use it to assign the file contents to a Python variable, then we won't have to deal with that file for the rest of the code.


```python
with open("test.txt", mode="r") as f:
    f_lines = f.readlines()
```


```python
f_lines
```




    ['Hello World!\n',
     'This is a random file\n',
     'With random lines of text\n',
     'For reading into Python\n']



# Importing libraries

External libraries are really useful in Python and data science. To be able to use them, we'll need to import those libraries in Python (whether in a Jupyter Notebook or in terminal). And to do that, we'll use the `import` keyword


```python
# Python has an external library called math, let's import it
import math
```

Once you've imported a library, you can run tab completion on it to see what functions are available.
The help shortcuts (`shift + tab + tab` and `shift + tab + tab + tab + tab`) will be available for these functions too.


```python
math.factorial(4)
```




    24




```python
math.sqrt(16)
```




    4.0



We can also just import certain functions using the `from` keyword. (Tab completion is generally available here too.)


```python
from math import sqrt
```

Now instead of calling `math.sqrt(number)`, we can just call `sqrt(number)`.

Another method of importing functions from a library is `from (library) import *` (e.g. `from math import *`). If you do it this way, the functions will be available by name without requiring `math.` in front of it.

There are two separate schools of thought regarding `from (library) import *`. Some people do it, and some people think it's bad practice. They say it's dangerous because then you could name some variable with the same name and it could overwrite that function.

Personally, I don't use `from (library) import *` because I rely on tab completion a lot. If I just did `import (library)`, I can use tab completion to scroll through all the functions available in that library later on when I need to look for something. I think it's a personal preference issue, just be consistent.

We can also provide aliases to the libraries we're importing, and most of the time, there's a convention to these aliases. We'll go through them as we go along, but let's look at how to do it with NumPy.


```python
# Syntax: import (library) as (alias)
import numpy as np
```

If the above didn't work for you, go to terminal and run `conda install numpy`. Certain libraries would have been installed for you when you installed the Anaconda distribution, but some other libraries have to be installed. Usually I start with `conda install (library)`, and if that doesn't work, I'll try `pip install (library)`, and if that doesn't work either, google it. There'll be an answer somewhere out there on the web. If not, [drop me an email](mailto:ongjoce@gmail.com) and I'll try to figure it out!

Now once we do that, instead of `numpy.`, we'll use `np.`.


```python
# Use the random function in numpy to generate a random integer between 0 (inclusive) and 10 (exclusive)
np.random.randint(0,10)
```




    8



# Practice

Let's try importing some of the libraries we'll be using later on.

- pandas, alias pd
- matplotlib.pyplot, alias plt

# Up next

And that's it for basic Python! We went a little fast and mostly had only one or two examples for each topic, but we'll be using it a lot as we go along and get our practice there. If you'd like a more in depth look into Python and more practice, I learnt it on [Coursera](https://www.coursera.org/learn/python) and by following the exercises [here](https://learnpythonthehardway.org). They might take you a while to run through but you will have a good handle on Python once you're done with them.
