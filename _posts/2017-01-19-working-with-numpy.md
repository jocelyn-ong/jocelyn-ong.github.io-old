---
tags:
  - data-science
  - data-science-notes
  - python
  - numpy
---
Data Science Notes Series: NumPy

{% include toc %}

# NumPy


```python
# To use numpy, we must import it first
import numpy as np
```

[NumPy](http://www.numpy.org){:target="_blank"} is a very useful module and we'll be using it a lot in our data science projects (or at least I did). Some of its functions are similar to what we have in vanilla Python, but more powerful.

We'll just look at a couple of my most-used functions now, but take some time to explore the functions available in NumPy.

# Similarities and differences

## Arrays

NumPy arrays are very much like Python lists, but arrays can only take elements of the same type.

To make an array, we use `np.array(object)`, where the object you pass in could be a Python list.


```python
np.array([1, 2, 3, 4, 5])
```




    array([1, 2, 3, 4, 5])




```python
# What if we pass in a list with mixed types?
np.array([1, 2.0, 3, 4, 5])
```




    array([ 1.,  2.,  3.,  4.,  5.])




```python
np.array([1, 2.0, "three", 4, 5])
```




    array(['1', '2.0', 'three', '4', '5'],
          dtype='<U32')



Notice that if we pass in a list with floats and integers, calling `np.array()` converts all the integers to floats, and if we pass in a list with floats, integers and strings, everything becomes a string.

The array forces elements into the lowest common level possible, and integers > floats > strings.

## `np.arange(start, stop, step)`

Remember `range` in Python? NumPy has a function similar to the Python `range` called `arange`, but it's more 'powerful' and I usually use `arange`.

Differences:

- `range` does not generate/ return a list - instead it returns an iterator which you can then use to generate a list
- `arange` will return a numpy array
- `range` can only take integers, `arange` can take floats
    - This is the main reason I use `arange` instead of `range`


```python
# Vanilla python range
range(5)
```




    range(0, 5)




```python
# Making a list with vanilla python range
[i for i in range(5)]
```




    [0, 1, 2, 3, 4]




```python
# Numpy's arange
np.arange(5)
```




    array([0, 1, 2, 3, 4])




```python
# steps in 0.5 instead of 1
np.arange(0,5,0.5)
```




    array([ 0. ,  0.5,  1. ,  1.5,  2. ,  2.5,  3. ,  3.5,  4. ,  4.5])



# `np.random`

NumPy has a `random` module (sub-module?) that I use a lot in simulation. It's similar to the `random` module which you can import separately but since numpy has it, I usually just use the numpy one.

A point to note: most computed random numbers are not random, they are also known as pseudorandom numbers and many of them can be replicated if we knew the seed. There is a way to generate 'real' random numbers, using a [quantum random number generator](https://www.google.com/search?q=quantum+random+number+generator){:target="_blank"}. It's outside my scope now, but there are various resources on the web available on it.

## `np.random.rand(shape)`

`np.random.rand()` generates a random float from 0 (inclusive) to 1 (exclusive). You can pass in optional arguments for the shape of your output.

- No arguments: a single float is returned
- One argument (X): an array of X elements is returned
- Two arguments (X, Y): an array of (X arrays of Y elements) is returned
- Three arguments (X, Y, Z): an array of [X arrays of (Y arrays of Z elements)] is returned
- And the list goes on


```python
# Random float from 0 (inclusive) to 1 (exclusive)
np.random.rand()
```




    0.21983316434418465




```python
# 5 random floats from 0 (inclusive) to 1 (exclusive)
np.random.rand(5)
```




    array([ 0.29558982,  0.62778646,  0.38066462,  0.52334871,  0.1963441 ])




```python
# 6 random floats from 0 (inclusive) to 1 (exclusive)
# in a 2 rows by 3 columns matrix
np.random.rand(2, 3)
```




    array([[ 0.4541702 ,  0.38452162,  0.5356184 ],
           [ 0.90509357,  0.78034629,  0.68994035]])




```python
# 24 random floats from 0 (inclusive) to 1 (exclusive)
# in 2 sets of 3 rows by 4 columns matrix
np.random.rand(2, 3, 4)
```




    array([[[ 0.67638188,  0.43869489,  0.68066697,  0.75200464],
            [ 0.25403872,  0.50745087,  0.52650444,  0.20771072],
            [ 0.41031867,  0.94924505,  0.35307978,  0.46142425]],

           [[ 0.8609553 ,  0.87694736,  0.69862799,  0.99265344],
            [ 0.02008121,  0.39655622,  0.16959415,  0.6304517 ],
            [ 0.32346767,  0.29279321,  0.28416981,  0.54016135]]])



## `np.random.randint(low, high, shape)`

`np.random.randint(X)` generates a random integer from 0 (inclusive) to X (exclusive). You can also specify the lower bound and the shape of the output.


```python
# Random integer from 0 (inclusive) to 5 (exclusive)
np.random.randint(5)
```




    1




```python
# Random integer from 5 (inclusive) to 15 (exclusive)
np.random.randint(5,15)
```




    8




```python
# 2 random integers from 5 (inclusive) to 15 (exclusive)
np.random.randint(5,15, 2)
```




    array([10, 10])




```python
# 6 random integers from 5 (inclusive) to 15 (exclusive)
# in a 2 row by 3 column matrix
np.random.randint(5, 15, (2,3))
```




    array([[12,  7, 12],
           [ 9, 10, 11]])



## `np.random.choice(object)`

`np.random.choice(object)` takes a list or an array, and picks an element out from it 'randomly'.


```python
np.random.choice(np.arange(0,10,0.1))
```




    4.5




```python
np.random.choice([1, 2.0, 3, "four"])
```




    '3'



Note that the random choice returned here is '3' instead of 3. Numpy converted your list to an array before it chose an random element, and because of that, 3 was converted from an integer to a string.

# `np.nan`

I'd like to introduce the concept of a null value here - it's not 0, 0 is a value, a null value can be considered a missing value. This will be important when we start to look at data sets.

In numpy, `np.nan` is a null value. The vanilla Python sort-of equivalent would be a None.


```python
None
```


```python
type(None)
```




    NoneType




```python
np.nan
```




    nan




```python
type(np.nan)
```




    float



# Practice

Let's take a look at the `shuffle` function in `np.random`.


```python
# Create an array of 10 random floats

# Sort your array in descending order

# Using the shuffle function, shuffle your array again

```

# Up Next

We're moving a little away from coding in our next post(s), and taking a look at statistics and probability. They are important if we want to understand our data.

This notebook with solutions can be viewed [here](http://nbviewer.jupyter.org/github/jocelyn-ong/data-science-notes/blob/master/content/05-numpy/solutions/0501-working-with-numpy-solutions.ipynb){:target="_blank"}.
