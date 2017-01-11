---
tags:
  - data-science
  - data-science-notes
  - python
---
Data Science Notes Series: Introduction to Python

{% include toc %}

# Introduction to Python

Yes! We've finally reached Python! Many people call Python a general purpose programming language, and it's great for data science because of the many packages and libraries available that work with it. Personally, I like Python because it's easy to learn. I hope you have a copy of this notebook to follow along - the easiest way to do this is to [clone my repo](https://github.com/jocelyn-ong/data-science-notes){:target="_blank"} (so that every time I update it, you can pull the updates from GitHub directly).

Alternatively, you can code along in terminal as well. To access Python in terminal, simply run `python` in terminal - you should see your prompt change to `>>>`. To exit, run `quit()`.

If you do decide to clone my repo, remember to rename your notebook before working in it, else if I make any changes to that file, it's going to overwrite whatever work you've done.

Steps for cloning:

- Create a your own repo on your GitHub account (or you can also fork this repo to your own account)
- _Clone **this** repo (not the one you created or forked)_
- Add a git remote link called upstream to **this** repo
    - `git remote add upstream LINK`
- Change your git remote origin link to your own repo or your fork
    - `git remote set-url origin LINK`
- _Every time you want to update your local folder, run `git pull upstream master` in terminal (in that folder) to get updated content that I have added_
- _Create a copy of the file you want to work on (make sure you rename it too)_
- When you're ready to update your own repo, run through `add`, `commit` and `push` to origin

Note: If you just want to work on the files without hosting them on your GitHub, just run the steps in _italics_ and replace `upstream` with `origin`.

# Python 2 or Python 3

There used to be some debate on whether we should use Python 2 or Python 3. I did all my Python courses with Python 2, but I think we should use Python 3. The differences aren't big (for now), and [this article](http://cyrille.rossant.net/why-you-should-move-to-python-3-now/){:target="_blank"} provides a great view of why we should be using Python 3. Also, the latest version, Python 3.6, was [just released recently](http://thenewstack.io/new-python-3-6-squeezes-dynamic-shifts-into-an-untyped-ecosystem/){:target="_blank"}!

You might run into some issues with Python 3, like unsupported libraries, but there's always some way to get around them.

# Comments

In Python, comments are denoted by a hash `#`. Placing a `#` in front of text tells Python not to execute that line.


```python
# This is a comment in a code formatted cell.
```

# Print

Remember how we did `echo TEXT` in terminal and whatever text you typed after `echo` would get 'printed' to the terminal? We can do that with Python using the `print` statement.

Syntax: `print ("TEXT")`


```python
print ("Hello World!")
```

Note that if you're running your code in a Jupyter notebook, sometimes a `print` statement might not be required - a result that is returned can also be displayed as an output.

# Strings, integers, floats, and booleans

There are four main types of variables we need to know:
- Strings: words, letters, anything that is wrapped between quotes or double quotes
- Integers: whole numbers (without quotes)
- Floats: decimal numbers (without quotes)
- Booleans: True or False (without quotes)
    - Note: True or False can be converted to numbers (F = 0)

The `type` statement will tell you what type a variable is.


```python
print (type("Hello World!"))
print (type("Two"))
print (type("2"))
print (type(2))
print (type(1.4))
print (type(True))
```

    <class 'str'>
    <class 'str'>
    <class 'str'>
    <class 'int'>
    <class 'float'>
    <class 'bool'>


## Basic string operations


```python
len("Hello World!")
```




    12



Notice that `len` includes spaces and punctuation in returning the number of characters in a string.


```python
"Hello World"[:5]
```




    'Hello'



Square brackets are used to slice strings (read: get some part of the string). In Python, each character is indexed (read: has a reference number attached to it), and this index starts from 0. So in "Hello", character 0 is "H" and 4 is "o".
- If you want to start from 0, you don't need to specify it
- Syntax: [start:end]
    - start: inclusive
    - end: exclusive
- If you want to end at the last letter, you don't need to specify it too
- You can also use negative numbers, -1 refers to the last letter, and it moves backwards from there


```python
"Hello World"[5:]
```




    ' World'




```python
"Hello World"[-10:-1]
```


    'ello Worl'

# Assigning variables

`=` allows you to assign a name to variables. For example, I could assign the name `x` to "Hello World", so that every time I run `x`, "Hello World would be returned.

```python
x = "Hello World"
```

# Math with Python: `+, -, *, **, /, //, %, ()`

## Add


```python
2 + 3
```




    5



Addition works with strings too!


```python
"Hello" + "World"
```




    'HelloWorld'



Note that it doesn't add a space in between each string, we'll have to do that on our own!


## Subtract


```python
4 - 2
```




    2



Do you think subtraction will work with strings? Try it out!


```python

```

## Multiply


```python
4 * 6
```




    24



What happens if we try to multiply strings?


```python
"Hello" * 3
```




    'HelloHelloHello'



## Power


```python
2 ** 2
```




    4




```python
2 ** 5
```




    32





## Divide

Division is a little different from the above three symbols - there are two different types in Python 3. `/` stands for decimal division (or just normal division as we know it).


```python
36 / 9
```




    4.0




```python
30 / 9
```




    3.3333333333333335



`//` stands for integer division, it only returns integers as answers, and it always rounds down.


```python
30 // 9
```




    3



## Modulo

The `%` sign in Python is referred to as the modulo. It returns the remainder of a number divided by another number.


```python
5 % 3
```




    2




```python
6 % 3
```




    0




```python
5 % 6
```




    5



## Parenthesis

Parenthesis in Python works in the same way as in math. It groups operations together and analyses them first before any other part of the equation. Otherwise, everything is evaluated from the left to the right like in math.

# Comparators: `>, <, >=, <=, ==, !=`

Comparators return a boolean, which can be useful as we'll see later on.

## Greater than, greater than or equal to


```python
3 > 2
```




    True




```python
10 >= 10
```




    True



## Smaller than, smaller than or equal to


```python
3 < 2
```




    False




```python
6 <= 6
```




    True



## Equal to

Recall that we used `=` for assigning names to variables. This means that we can't use it as a comparator. In Python, we use `==` (double equals) to check if two things are equal.


```python
5 == 4
```




    False




```python
"Hello" == "Hello"
```




    True




## Not equal to

In Python, we use `!` to symbolize `not`, so `!=` means not equal to.


```python
5 != 4
```




    True




```python
"Hi" != "Hello"
```




    True



# More than one condition

Now we know how to check if `3 > 2`, what if we want to know if `3` is also greater than `2 * 4`?


```python
3 > 2 and 3 > 2 * 4
```




    False



How did that work? `2 * 4` gives `8`, so `3 > 8` returns `False`. The `and` operator requires that all terms are `True` before it returns `True`. In this case, we had `True and False`, so the result is a `False`.


If you just need at least one of the conditions to be `True`, the `or` operator will do that for you.


```python
4 > 2 or 3 > 5
```




    True



# Practice

- Assign the sentence "Hello Python" to the a variable `a`.
- Print `a`.
- Print `a` 5 times without spaces between each repeat.
- Using at least 2 comparators and the `and` operator, output the boolean `True`.
- Using at least 2 comparators and the `or` operator, output the boolean `True`.

# Up next

In our next post(s), we'll look at groups of variables, conditionals, and working with external files.

The notebook to this post can be found [on my repo](https://github.com/jocelyn-ong/data-science-notes){:target="_blank"} and the  notebook with solutions can be viewed [here](http://nbviewer.jupyter.org/github/jocelyn-ong/data-science-notes/blob/master/content/04-basic-python/solutions/0401-introduction-to-python-solutions.ipynb){:target="_blank"}.
