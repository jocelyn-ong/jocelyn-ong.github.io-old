---
tags:
  - data-science
  - data-science-notes
  - stats
  - python
  - numpy
  - scipy
  - pandas
---
Data Science Notes Series: Basic Statistics

{% include toc %}

# How much statistics do we need?

## What is statistics?

> [Statistics is the study of the collection, analysis, interpretation, presentation, and organization of data.](https://en.wikipedia.org/wiki/Statistics)

To (over)simplify it, statistics is looking at our data and understanding what it's trying to say.

## Why do we need to know statistics?

Statistics is an important part of data science. It's how we understand and test our data. That said, we probably don't need a PhD in statistics to be able to understand our data. We need some basic understanding of statistics, and we need to know how to google any more advanced questions we may have in the future.

One thing I learned during my Data Science part time and Data Science Immersive, learning how to find your answers when questions pop up is much more important than attempting to cram every bit of information into your brain right from the start.


```python
import numpy as np
from scipy import stats
import pandas as pd
```


```python
some_numbers = [np.random.randint(1,6) for _ in np.arange(10)]
```


```python
print (some_numbers)
```

    [2, 4, 3, 5, 5, 2, 3, 4, 4, 3]


Before we actually do anything with our data, we want to look at its characteristics.

We generated a sample data set of 10 numbers above. Using this data set, we'll look at how to describe our data. This is also probably a good time to introduce SciPy and Pandas, two other python modules. We imported them above together with numpy, and we'll take a look at how we're going to use them later.

# Min and Max

Some of the easiest questions we can ask: What is our range? What's the smallest number? What's the biggest number?


```python
# Finding our smallest number or minimum with vanilla Python
min(some_numbers)
```




    2




```python
# Min with numpy
np.min(some_numbers)
```




    2




```python
# Finding our largest number or maximum with vanilla Python
max(some_numbers)
```




    5




```python
# Max with numpy
np.max(some_numbers)
```




    5



# Mean, Median, Mode

Our next question might be: what is the average of our data?

But first, what is average?

For most of us, average means the sum of all your numbers divided by the number of numbers you have. In statistics, this is the mean. But sometimes when we say average, we might not be talking about the mean.

In statistics, when we say average, usually we mean 'measure of central tendency'. And central tendency - central or typical value - that could mean any one of these:

- Mean
    - Sum of numbers divided by number of numbers
- Median
    - The middle value of all your numbers
    - To find the median, you first have to sort all your numbers
    - If you have an odd number of numbers, it's the middle number
    - If you have an even number of numbers, it's the mean of the two middle numbers
- Mode
    - The most commonly occurring number

## Mean


```python
# Mean using vanilla python
# mean = sum / quantity
my_mean = sum(some_numbers) / len(some_numbers)
print (my_mean)
```

    3.5



```python
# Mean using numpy
my_easy_mean = np.mean(some_numbers)
print (my_easy_mean)
```

    3.5


## Median


```python
# Median using vanilla python
# We don't actually use python to find the median directly
# We use it sort and also to find the mean of our middle numbers
sorted_numbers = sorted(some_numbers)
print (sorted_numbers)
```

    [2, 2, 3, 3, 3, 4, 4, 4, 5, 5]



```python
my_median = (sorted_numbers[4] + sorted_numbers[5])/2
print (my_median)
```

    3.5



```python
# Median using numpy
my_easy_median = np.median(some_numbers)
my_easy_median
```




    3.5



## Mode

There's no mode using vanilla python. Since mode is just the most commonly occurring number, we just need to count each number and take the highest one.

That looks easy if we have just 10 numbers, but what if we have 100?

For this, I like to use a function called `Counter` from a module called `collections`.


```python
from collections import Counter
```


```python
number_count = Counter(some_numbers)
```


```python
# Counter returns an object with key-value pairs
# where key is whatever you were counting,
# and value is the number of times it appeared
number_count
```




    Counter({2: 2, 3: 3, 4: 3, 5: 2})




```python
# Now I can use the .most_common(X) method to
# return the X most commonly occuring number in the format
# (number,times_it_occurs)
number_count.most_common(1)
```




    [(3, 3)]




```python
# But what if I had multiple modes?
# (Also known as multi-modal distribution)
# Just in case I do, I'm going to loop through all my numbers
# And compare the number of times it appears
# To the max count that some number appears in my data set
# And if it matches, I print out that number
for i in number_count.keys(): # Loop through all numbers
    # Check if the number of times it appears matches the max
    if number_count[i] == max(number_count.values()):
        print (i) # If yes, print it
```

    3
    4


There's no easy way to find mode using numpy either, which was why we imported scipy up above.


```python
# Mode using scipy.stats
my_easy_mode = stats.mode(some_numbers)
print (my_easy_mode)
```

    ModeResult(mode=array([3]), count=array([3]))


# Quantiles

Another way to describe our data is by quantiles. Quantiles are sort of like dividing your data into sections. Like when you take standardized test, sometimes they tell you that your score was in the 90th percentile - it means you did better than 90% of the people who took the test.

Another word we may use is quartiles - quartiles refer to each portion of 100% quartered, i.e. 25%, 50%, 75%, also known as first, second, and third quartile respectively.

The 50% percentile is also known as the second quartile, and the median.

Pandas has a quantile function that will allow us to find any quantile we want, but we'll have to convert our list/ array to a pandas series first.


```python
# Convert list to series
number_series = pd.Series(some_numbers)
```


```python
# Getting our 50th percentile, or median
number_series.quantile(0.5)
```




    3.5




```python
# Getting our 25th percentile
number_series.quantile(0.25)
```




    3.0




```python
# Getting our 75th percentile
number_series.quantile(0.75)
```




    4.0



# Practice


```python
practice_set = [np.random.randint(1,101) for _ in range(100)]
```

I've created a list of 100 random (or pseudorandom) numbers above. Using that as your practice set and any method you like, find the:

- Min
- Max
- Mean
- Median
- Mode
- First quartile
- Third quartile

# Up Next

Here's our road map for statistics (which I will hopefully follow):

- Probability and distributions
- Hypothesis testing
- P-values and confidence intervals
- T-test
- Chi-squared test

This notebook with solutions can be found [here](http://nbviewer.jupyter.org/github/jocelyn-ong/data-science-notes/blob/master/content/06-basic-statistics-and-probability/solutions/0601-basic-statistics-solutions.ipynb).
