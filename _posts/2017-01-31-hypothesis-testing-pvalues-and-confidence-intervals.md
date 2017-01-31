---
tags:
  - data-science
  - data-science-notes
  - stats
---
Data Science Notes Series: Hypothesis Testing, P-values and Confidence Intervals

{% include toc %}

# Questions and answers

So what is data science?

For me, I think it's about answering questions with data. It's about looking at your data and understanding it, then taking that and finding an answer to a question. Sometimes, we have an open ended question - and it's difficult getting an answer to that. We need to rephrase our question so that it takes a yes or no answer, and sometimes it means that we need to make some assumptions.

Take a look at the xkcd comic below.

[![xkcd-sig](http://imgs.xkcd.com/comics/significant.png)](http://xkcd.com/882/)

The original question might have been "What causes acne?", which is open ended. You could look at tons of data and you might not get an answer. The first frame takes care of this - "Jellybeans cause acne!" Now what we're testing is if that statement is true or false - also known as hypothesis testing. There could be a million other things that cause acne, but we don't care, we only want to know if jellybeans cause acne.

Then you see (p > 0.05) and (p < 0.05) - they're talking about p-values, which we will go into a little bit further down. And when we talk about p-values, I guess we also have to talk about confidence intervals.

# Hypothesis Testing

In hypothesis testing, we need a null hypothesis ($H_0$) as well as an alternative hypothesis ($H_1$). The null hypothesis is the statement that states that there is no relationship between what you're testing and the outcome. So in our jellybean example:

$H_0 = "There\ is\ no\ relationship\ between\ jellybeans\ and\ acne."$

And the alternative hypothesis is the 'opposite' of the null hypothesis.

$H_1 = "There\ is\ a\ relationship\ between\ jellybeans\ and\ acne."$

And the statement that we're trying to prove or disprove is $H_0$. The alternative hypothesis is usually the more difficult statement to prove, but if we can disprove $H_0$, then there's a chance that $H_1$ might be true. (Note that we don't say that just because $H_0$ is false, therefore $H_1$ is true.)

# P-value

> [The P value is used all over statistics, from t-tests to regression analysis. Everyone knows that you use P values to determine statistical significance in a hypothesis test. In fact, P values often determine what studies get published and what projects get funding.](http://blog.minitab.com/blog/adventures-in-statistics-2/how-to-correctly-interpret-p-values){:target="_blank"}

The thing about experiments is that there's always a chance that you don't get the result you're expecting, even if you know for sure what the result is supposed to be.

For example, let's look at our jellybeans and acne case. The p-value in that case is the chance that you will find a relationship between jellybeans and acne given that that is no actual relationship between the two. We want this value to be as small as possible, and when it's small enough, we say that our result is significant. _When we say significant, we mean that it is unlikely that we got that result by chance._

But what is small enough? It could be any number really, but one of the 'standards' many people use is 0.05 or 5%. 5% was a figure that was set by [Ronald Fisher](https://www.famousscientists.org/ronald-fisher/){:target="_blank"} and it's a widely used standard now. But you could go lower, it's really up to you to decide what's significant to you. (I don't think most people go higher than 5% though.)

## Calculating p-values

Let's look at a simpler example (instead of jellybeans and acne).

Flip a coin. Assuming it's not a trick coin, you've got either a heads or a tails. Let's say we flipped the coin 30 times, and got 20 heads. Now we want to test if the coin is a fair coin.

Our null hypothesis, $H_0$, is that the coin is fair. And the alternative hypothesis, $H_1$, is that it's a biased coin.

If the coin is fair, we would expect the chance of getting heads and tails to be 50-50. (Note that this doesn't mean that if we flipped a coin 30 times, we would always get 15 heads and 15 tails - but if we did it 10000 times, and repeated that 10000 times, we'd expect to see a normal distribution on the number of heads we got, with its peak being around 5000.)

So to test $H_0$, we need to set up an experiment. We need to flip the coin 30 times, and repeat that many times (say e.g. 10000). Each repeat is a trial, and for each trial, we record the number of heads we get.

Let's simulate that.


```python
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```


```python
# We simulate a coin flip by generating a random number between 0 and 1
# If the number is below 0.5, we'll call it tails, otherwise, we'll call it heads
# We want the number of heads per 30 flips
one_trial = sum([1 if i > 0.5 else 0 for i in np.random.random(size=30)])
one_trial
```




    17




```python
# Now we want that repeated 10000 times
many_trials = [sum([1 if i > 0.5 else 0 for i in np.random.random(size=30)]) for _ in range(10000)]
```


```python
# Now we plot the distribution of our many_trials
# Introduction to seaborn!
# It's based on matplotlib, and looks pretty
# It has some of the same basic plots as matplotlib
# and we're going to use it to plot a histogram now
sns.distplot(many_trials, kde=False, bins=10);
plt.show();
plt.savefig("seaborn_distplot.png")
```

[![seaborn_distplot]({{ site.url }}{{ site.baseurl }}/images/ds-notes/stats/seaborn_distplot.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/stats/seaborn_distplot.png)



```python
# Calculate p-value
# We want the probability of getting at least 20 heads in 30 coin flips

# Find number of trials that had 20 heads or more
at_least_20 = sum(np.array(many_trials) >= 20)
at_least_20
```




    478




```python
# Probability of getting 20 heads is number of trials that had 20 heads/total number of trials
pval = at_least_20/len(many_trials)
pval * 100
```




    4.7800000000000002



In the above example, our p-value is 4.78%. Meaning to say, if the coin we flipped earlier was fair, there was only a 4.78% chance that we would get 20 heads out of 30 (i.e. low). It is low enough that we can reject $H_0$ and say that the coin is not fair.

## Problem with p-values

The problem with p-values is that they change.


```python
pvals = []
for _ in range(500):
    many_trials = [sum([1 if i > 0.5 else 0 for i in np.random.random(size=30)]) for _ in range(10000)]
    pvals.append(sum(np.array(many_trials) >= 20)/len(many_trials))
```


```python
sns.distplot(pvals, kde=False);
plt.show();
plt.savefig("pval_dist.png");
```

[![pval_dist]({{ site.url }}{{ site.baseurl }}/images/ds-notes/stats/pval_dist.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/stats/pval_dist.png)
![png](output_20_0.png)


Look at that! We got a range of p-values, and some of them were above 0.05! Granted, it looks like you're more likely to get something below 0.05, so it's more likely that you would reject the null hypothesis than fail to reject it. But it's there, there's a chance that you might have gotten a p-value above 0.05 and failed to reject the null hypothesis.

## Other methods of calculating p-values

Apart from conducting an experiment like above, there are other methods of calculating p-values, e.g. using the t-statistic and chi-squared methods. I wanted to go over those in a little bit more detail in later posts, so just know for now that there's more than one way to calculate p-values.

# Confidence Intervals

> A confidence interval is a range of values that is likely to contain an unknown population parameter. If you draw a random sample many times, a certain percentage of the confidence intervals will contain the population mean. This percentage is the confidence level.

> Most frequently, you’ll use confidence intervals to bound the mean or standard deviation, but you can also obtain them for regression coefficients, proportions, rates of occurrence (Poisson), and for the differences between populations.

> Just as there is a common misconception of how to interpret P values, there’s a common misconception of how to interpret confidence intervals. In this case, the confidence level is not the probability that a specific confidence interval contains the population parameter. [(Source: Minitab)](http://blog.minitab.com/blog/adventures-in-statistics-2/understanding-hypothesis-tests:-confidence-intervals-and-confidence-levels){:target="_blank"}

- A confidence interval is a range of values
- This range of values may or may not contain a value that you're looking at (usually the population mean)
- There's a confidence level associated with confidence intervals (e.g. 95%)
- The confidence level (e.g. 95%) does not indicate the probability that this range will contain the value you're looking at
- The confidence level (e.g. 95%) indicates the proportion of samples picked from a population that will contain the value you're looking at
    - e.g. If you picked 100 samples out of a population, 95 of them will contain the true population mean.
- A higher confidence level (e.g. 90%) means that more samples will contain the true population parameter.
    - This also means that the range of the confidence interval is likely to be wider.
- If we don't see the null hypothesis value within the range set by the confidence interval, it means that it is a significant result (i.e. it is unlikely that you would have gotten your experiment result given that the null hypothesis is true).


# Up Next

This was just a cursory look at hypothesis testing, p-values, and confidence intervals. I would definitely encourage further reading into this, and [minitab](http://blog.minitab.com/blog/statistics-2){:target="_blank"} has some great articles on the subject. I'm going to move on to data exploration in our next notebook/ posts, but I want to revisit statistics and come back to the t-statistics and the chi-squared methods.

In the meantime, [google](https://www.google.com/){:target="_blank"} is my best friend (and yours) and keep reading!
