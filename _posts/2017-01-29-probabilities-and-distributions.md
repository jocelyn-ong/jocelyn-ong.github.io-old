---
tags:
  - data-science
  - data-science-notes
  - stats
---
Data Science Notes Series: Probabilities and distributions

{% include toc %}

# Probabilities and distributions

Now that we know the basic statistics descriptions, we'll start looking at probability and distributions. We'll also look at plotting data on a chart so that we can visualize the distributions.


```python
# We start by importing the libraries that we need
import numpy as np
from scipy import stats
import pandas as pd
import matplotlib.pyplot as plt
```

# Probability, expected value and odds

## Probability

When we talk about probability, we're referring the likelihood of an event happening (loosely speaking).

For example, the probability of getting heads on the toss of a fair coin is 0.5. How did we get that? First, we ignore the possibility that it could land on its edge, so our possible outcomes are either heads or tails. Assuming the coin is fair, our outcome should be split equally between the two possibilities - so 50% heads and 50% tails. And 50% as a float is just 0.5.

Or take a fair, six-sided die that's numbered one to six, one number on each side (i.e. a normal die that most of us are familiar with). The probability that you'll get a four on a roll is 1/6.

The notation for probability is `P(event)`. So in our examples above, it would be `P(heads) = 0.5` and `P(four) = 1/6`.

## Expected value

I used to confuse expected value with probability, I thought they were the same thing called differently. But they're not.

When we talk about probability, we're generally just talking about one test. The expected value is more for a bunch of tests (sort of).

An example might be more helpful. Imagine you're taking a test with 100 questions, and each question has four options, A to D. Let's pretend you don't know ANY of the answers, and you're going to guess. Each question has only one right answer, and the probability of you getting a question correct is 25% or 1/4 (assuming your teacher isn't pranking you and there's an equal chance of any option being the correct answer).

So `P(correct) = 0.25`. Your expected value of the test whole test, assuming each question is worth one mark, is then 25 marks, which is `number of marks * probability of answer being correct` for each question.

The notation for expected value is `E(group of events)`. In our example, one event is getting one question correct, and our group of events is the total test.

To put expected value in terms of probability in our example, `E(test score) = Sum (marks * P(correct)) for all questions in the test`.

## Odds

Now odds is an odd thing (pun intended). Again, I thought it meant the same thing as probability - as in I thought the probability of something happening and the odds that something would happen were the same things. Wrong again. But they are related.

Odds can be expressed as a ratio or as a number.  In a fair coin, the probability of getting heads is 0.5. And the probability of not getting heads is also 0.5. The odds of getting a head is `P(heads) : P(not heads)`, which after all the math, translates to `1:1`, or just 1.

Maybe using a die is easier. The odds of getting a one is `P(one) : P(not one)`, or 1/6 : 5/6, finally leaving us with `1:5`.

# Distributions

When we look at our data, one of the questions we'd ask is what is the distribution of the data?

What does that even mean?

> [A probability distribution is a statistical function that describes all the possible values and likelihoods that a random variable can take within a given range. This range will be between the minimum and maximum statistically possible values, but where the possible value is likely to be plotted on the probability distribution depends on a number of factors. These factors include the distribution's mean, standard deviation, skewness and kurtosis.](http://www.investopedia.com/terms/p/probabilitydistribution.asp){:target="_blank"}

We want to see how our data looks like on a chart. Along the x-axis, we'll have the possible values, and on the y-axis, the chance of getting that figure.

## Uniform distribution

A uniform distribution is one where the probability of getting any value is the same for all values in the range.

What that means is that, when I try to pick a sample out of the bag of say, numbers 1 to 100, there's an equal chance of getting any of those numbers. So if I pick many many times, I should get a fairly equal number of picks of each number.

Numpy's random module has a `uniform` function that will generate a random variable that's pulled from a uniform distribution. That is to say, if we pull many of such samples and plot them, we should get a fairly straight line for count.


```python
# Generate data
uniform_values = pd.Series(np.random.uniform(size=10000))

# Plot the count of our data points using matplotlib
# We want a histogram of our data
# Since our data is in a pandas series
# we can just call .hist()
uniform_values.hist();
plt.show();
plt.savefig("uniform_dist.png");
```

[![uniform_dist]({{ site.url }}{{ site.baseurl }}/images/ds-notes/stats/uniform_dist.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/stats/uniform_dist.png)



## Normal distribution

The normal distribution is probably one of the most important distributions we'll need to know. It is also commonly called the bell curve - or at least it was in my university when we were talking about grade moderation - because it is shaped like a bell.


```python
# Generate data
normal_values = pd.Series(np.random.normal(size=10000))

# Plot the count of our data points using matplotlib
normal_values.hist(bins=20);
plt.show();
plt.savefig("normal_dist.png");
```


[![normal_dist]({{ site.url }}{{ site.baseurl }}/images/ds-notes/stats/normal_dist.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/stats/normal_dist.png)


In a normal distribution, it's not equally likely for you to get each value within the distribution. There is a higher likelihood of getting the values closest to the mean, and as you get further and further away from the mean, the chance of getting a value decreases.

## Other distributions

There are MANY other distributions in the study of probability - Poisson, binomial, Bernoulli etc. - but I found that a lot of the time, we were dealing with the normal distribution, so I won't go over the others unless we come over them during the study of a data set.

# Variance and Standard Deviation

## Variance

Variance measures the spread of a distribution from its mean. To find variance:

- Find the mean of the data set
- For each value in the data set, find the difference between it and the mean of the set
- Square that difference
- Add up all those squares, then find the mean of those squares and that's the variance

With the variance, we can tell how spread out a data set is - the larger the variance, the larger the spread. It doesn't give the exact value of the values that are furthest away from the mean; it gives the 'average' or 'expected' distance of each value from the mean.

## Standard deviation

The standard deviation is the square root of the variance - it's how we bring the 'spread' back onto the same scale as our original data set.

# Approximations

## Central Limit Theorem

> [Roughly, the central limit theorem states that the distribution of the sum (or average) of a large number of independent, identically distributed variables will be approximately normal, regardless of the underlying distribution. The importance of the central limit theorem is hard to overstate; indeed it is the reason that many statistical procedures work.](http://www.math.uah.edu/stat/sample/CLT.html){:target="_blank"}

## Law of large numbers

> [The law of large numbers states that the sample mean converges to the distribution mean as the sample size increases, and is one of the fundamental theorems of probability.](http://www.math.uah.edu/stat/sample/LLN.html){:target="_blank"}

With the central limit theorem and the law of large numbers, when we have a large enough sample size, our data is going to be normally distributed, and if we pull enough of such samples from the population, our sample mean is going to approximate the population mean.

- Population: the entire group of whatever you're studying (e.g. all the students in a college)
- Sample: a subset of the population (e.g. students picked at random from the whole college)

# Up next

This was just a cursory look at probability and distributions, just enough for us to understand our data later on. It's probably ideal to get a better understanding of statistics as a whole as we get deeper into our data science journey, and we might look deeper into it at a later time. As always, if you feel like you need to know more about this before moving on, the web is full of readily available resources.
