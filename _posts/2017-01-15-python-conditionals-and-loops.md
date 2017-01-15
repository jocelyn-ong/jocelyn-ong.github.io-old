---
tags:
  - data-science
  - data-science-notes
  - python
---
Data Science Notes Series: Python conditionals and loops

{% include toc %}

# Conditionals and loops

Conditionals and loops are useful because with them, we can tell Python to:

- do something if a condition is true
- do something repeatedly for some number of times

It means that we don't have to check if the condition is true manually, and we don't have to repeat our code just to repeat our output.

# If, Elif, and Else

Syntax:


```python
# if condition_is_true:  
#     do_something  
# elif other_condition_is_true:  
#     do_something_else  
# else:  
#     do_another_thing  
```

**New note about Python - Indentations matter**  
Spaces mean something to Python, when you indent a line of code, it tells Python that it's part of the previous line. If a line of code ends with a colon `:`, the next line must be indented (by four spaces).

The above is called an `if` statement. Notes about `if` statements:

- You always start with an `if`
- An `if` always needs a condition to test
- `elif` stands for else if
    - It's optional
    - It also needs a condition to test
- `else` is the last part of an `if` statement
    - It does not need a condition to test
    - It tells Python what to do if all the conditions you wanted to test turn out False
    - It is also optional
        - If an `else` statement is not provided and the conditions tested are all False, Python does nothing
- The evaluation of statements start from the top. If one statement is True and is executed, the rest of the `elif`s and `else` will not run even if they would have been true.


```python
# Example if
if 5 > 2:
    print ("5 is bigger than 2.")
```

    5 is bigger than 2.



```python
# Example if-else
a = 3
if a > 5:
    print ("a is bigger than 5.")
else:
    print ("a is not bigger than 5.")
```

    a is not bigger than 5.



```python
# Example if-elif-else
b = 10
if b > 20:
    print ("b is bigger than 20.")
elif b > 12:
    print ("b is bigger than 12.")
elif b > 8:
    print ("b is bigger than 8.")
else:
    print ("b is not bigger than 8.")
```

    b is bigger than 8.



```python
# Example if-elif-else that seems to be in the wrong order
c = 20
if c > 3:
    print ("c is bigger than 3.")
elif c > 15:
    print ("c is bigger than 15.")
else:
    print ("c is not bigger than 3.")
```

    c is bigger than 3.


Our desired outcome was to get "c is bigger than 15.", but because the first part was True, the second part never got evaluated at all.

Can you rearrange the above to get the desired outcome?

# For Loops

Before we talk about `for` loops, we need to learn about the `range(start,stop,step)` function.

- `start` refers to the number to start counting from
    - This is inclusive
    - This is optional, if `start` is not provided, Python assumes you want to start from 0
- `stop` refers to the number to stop counting at
    - This is exclusive
    - This is required
- `step` refers to the difference between each number
    - This is optional, if `step` is not provided, Python assumes you want steps of 1

For example, `range(5)` means start from 0, count in steps of 1, and stop before 5 (i.e. at 4).

`for` loops are useful for telling Python to do something a set number of times.


```python
# For example, we want Python to print "Hello World" 5 times, on 5 different lines
for i in range(5):
    print ("Hello World")
```

    Hello World
    Hello World
    Hello World
    Hello World
    Hello World


`i` doesn't mean anything, it's just a placeholder - you can call it anything in this case, `i` is just commonly used.

This would be different if, say, you wanted Python to print the numbers 5 to 10.


```python
# Using a for-loop to print the numbers 5 to 10
for i in range(5,11):
    print (i)
```

    5
    6
    7
    8
    9
    10


Tip on printing: What if you wanted everything to be printed on the same line?


```python
# Using a for-loop to print the numbers 5 to 10 on the same line
for i in range(5,11):
    print (i, end=" ")
```

    5 6 7 8 9 10

This is different from Python 2, where `print i, ` would have worked. With Python 3, you have to tell the `print` statement exactly how you want each `printed line` to end.

`for` can also be used with lists.


```python
my_list = [1, 2, "three", 4.0, "five"]
for i in my_list:
    print (i)
```

    1
    2
    three
    4.0
    five


# While Loops

Before we move forward, **`while` loops are dangerous!** (We'll explain why in a moment, but remember to use them with caution.)

Syntax:


```python
# while condition_is_true:
#     do_something
```

Notice that in our syntax, it doesn't say when to stop? A `while` statement evaluates a condition, and if it's true, does something, goes back to evaluating the **same** condition, and it goes on and on and on...

So the way to stop a `while` loop, is to do something to the condition you're testing such that at some point, the condition becomes false.


```python
# For example
i = 0
while i < 5:
    print (i)
    i = i + 1
```

    0
    1
    2
    3
    4


The above statement looks at `i`, checks if it's smaller than 5, and if it is, it prints `i` and it increments `i` by 1. At some point, `i` is will be 5 or greater, and that's when the `while` loop stops.

__Tip: Instead of `i = i + 1`, we can do `i += 1`. This works for `-`, `*` and `/` as well.__

# List/ Dictionary Comprehension

List/ dictionary comprehensions are essentially `for` loops used to generate lists or dictionaries.


```python
# Using a normal for loop to generate a list of even numbers smaller or equal to 20
even_list = [] # Create an empty list
for i in range(1,21): # We use 21 because we want 20 to be included
    if i % 2 == 0: # Check if i is divisible by 2
        even_list.append(i) # If it is, append it to our list
print (even_list)
```

    [2, 4, 6, 8, 10, 12, 14, 16, 18, 20]



```python
# Using list comprehension to generate the same list
even_list_2 = [i for i in range(1,21) if i % 2 == 0]
print (even_list_2)
```

    [2, 4, 6, 8, 10, 12, 14, 16, 18, 20]


What a list comprehension does is it takes a `for` loop, moves the parts around and flattens it into one line.

A dictionary comprehension is similar.


```python
# Using a normal for loop to generate a dictionary where values are squares of the keys
square_dict = {}
for i in range(10):
    square_dict[i] = i ** 2
print (square_dict)
```

    {0: 0, 1: 1, 2: 4, 3: 9, 4: 16, 5: 25, 6: 36, 7: 49, 8: 64, 9: 81}



```python
# Using dictionary comprehension to generate the same dictionary
square_dict_2 = {i:i**2 for i in range(10)}
print (square_dict_2)
```

    {0: 0, 1: 1, 2: 4, 3: 9, 4: 16, 5: 25, 6: 36, 7: 49, 8: 64, 9: 81}


# Practice



test_number = 20

- Using an if statement, print "Hello World!" if test_number is greater than 15, otherwise, print "This is wrong."
  - Test it with test_number = 20, 10, and 5
- Use a for loop to print out only odd numbers from 1 to 10
- Using a combination of a while loop and an if statement, print out numbers that are either multiples of 3 or 5 and are smaller than 20
- Using only list comprehension and dictionary comprehension, generate a dictionary where keys are multiples of 5 from 5 to 30 inclusive, and values are lists of squares of positive numbers (including 0) smaller than the key e.g. {key:value} {5: [0,1,4,9,16]}



# Up Next

- Functions and classes
- External files and libraries

This notebook with solutions can be viewed [here](http://nbviewer.jupyter.org/github/jocelyn-ong/data-science-notes/blob/master/content/04-basic-python/solutions/0403-python-conditionals-and-loops-solutions.ipynb){:target="_blank"}.
