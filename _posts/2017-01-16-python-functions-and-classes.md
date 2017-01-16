---
tags:
  - data-science
  - data-science-notes
  - python
---
Data Science Notes Series: Python Functions and Classes

{% include toc %}

# Python Functions and Classes

We've been using Python functions and classes in the last two notebooks before we even really talked about them. In simple terms, a function is a block of code that does something. A class is a combination of variables, functions (aka methods), all grouped together, to form an object.

- A `list` is an example of an object/ class, and a `list` method (e.g. `.sort()`) is a function.
- Functions end with a parenthesis `()`. But not everything that ends with parenthesis `()` is a function.
- Functions require a `return` statement
    - Especially if you want to use the output of that code
    - Tip: If you're defining a function and you feel like you're going to type `print`, use `return` instead

# `def`

We can write our own functions in Python, so that we can reuse our code without having to rewrite it over and over again, and this is done with the keyword `def`.

Syntax:


```python
# def function_name(arguments):
#     code_for_your_function
```

Arguments are 'things' that your function uses in its code. They are optional, and they can also have a default value.


```python
# Example of a function that does not require arguments
def no_arg_function():
    return "This function does not need arguments."
```


```python
no_arg_function()
```




    'This function does not need arguments.'




```python
# Example of a function that requires two arguments
def two_arg_function(a, b):
    return a * b
```


```python
two_arg_function(3, 6)
```




    18




```python
# Example of a function that requires an argument, but has a default value
def one_default_arg_function(a=10):
    return [i for i in range(a)]
```


```python
one_default_arg_function(3)
```




    [0, 1, 2]




```python
one_default_arg_function()
```




    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]



- Notice that in the above function, we stated that it required one argument, and we assigned a value of `10` to it when we were defining the function.
- When we called the function with an argument of 3, it created a list up to but not including 3
- When we called the function without providing an argument, it used the default provided (i.e. 10), and created a list up to but not including 10

The above were simplistic functions with only one line of code and did only one thing. (And they were stupidly named - please give your functions better names.) Usually when we write a function, it's because we don't want to repeat a block of code, instead of replacing just a single line.


```python
# Example function that does more than one thing
def my_function(a,b):
    list_one = [i for i in range(a)]
    list_two = [i**2 for i in range(b)]
    both_lists = list_one + list_two
    add_two = [i+2 for i in both_lists]
    add_two.sort()
    return set(add_two)
```


```python
my_function(5,16)
```




    {2, 3, 4, 5, 6, 11, 18, 27, 38, 51, 66, 83, 102, 123, 146, 171, 198, 227}



# `lambda`

The `lambda` function is also known as the anonymous function. It used for those one-liners that don't need a named function.


```python
my_list = list(my_function(5, 16))
my_list.sort(key=lambda x: -x if x % 2 == 0 else x)
my_list
```




    [198, 146, 102, 66, 38, 18, 6, 4, 2, 3, 5, 11, 27, 51, 83, 123, 171, 227]



In the above cell, we used a lambda function to tell Python to sort our list in a special way: if a number is even, treat it as a negative number, otherwise, treat it as it is. You see that the result of our sort is that the even numbers are sorted in descending order, followed by the odd numbers in ascending order.

# Classes

- To create a class in Python, we'll use the `class` keyword.
- Classes have a special `__init__` method that tells Python how to initialize it
    - Methods which start and end with two underscores are special - they are 'hidden' methods and will not show up when you do tab completion
- The `self` keyword is also important in classes
    - It is used to refer to an instance of a class
    - All functions/ methods defined in a class must have `self` as the first argument


```python
# We'll create a class called Person
class Person:
    def __init__(self, firstname, lastname):
        # This means that when we create a new Person, we need to pass in the arguments firstname and lastname
        self.firstname = firstname
        self.lastname = lastname

    def __str__(self):
        # The __str__ method is what will be returned when you try to print an instance of an object
        return "Hello, my name is {} {}.".format(self.firstname, self.lastname)

    def get_initials(self):
        self.initials = self.firstname[0] + self.lastname[0] # Get first letters of firstname and lastname
        return self.initials
```


```python
# Now we'll create an instance of our Person class
me = Person("Jocelyn", "Ong")
```


```python
# You'll see that the variable me will now have me.firstname and me.lastname variables
# Note that me.firstname and me.lastname don't need parantheses
me.firstname
```




    'Jocelyn'




```python
# Now let's try printing the variable `me`
print(me)
```

    Hello, my name is Jocelyn Ong.



```python
# Let's generate my initials
me.get_initials()
```




    'JO'




```python
# Look at what's available to `me` now with tab completion
# me.initials didn't exist previously, we created it when we ran me.get_initials
me.initials
```




    'JO'



# Practice

For practice, we'll create a class with some functions.

- Create a class called Notebook
- Define the __init__ method to take one argument: subject
- Define the __str__ method to return "This is my (subject) notebook." and replace (subject) with whatever subject you typed
- Define a method called add_note that takes one argument that is a string and adds it to a dictionary called notes
    - The dictionary should have key-value pairs where the string argument is the value, and the keys are integers starting from 1, and increasing by 1 each time you add a new note
- Define a method called print_notes that prints out your notes line by line in numerical order of key
    - The format to be printed should be"  
    "1. This is your first note"  
    "2. This is your second note"  

Now create a new Notebook and add some notes to it!

# Up Next

One of the great things about Python is the number of external libraries and packages available. So far, we've been using just plain vanilla Python. Our next post/ notebook will look at how we import external libraries and also how to open up external files in Python.

This notebook with solutions can be viewed [here](http://nbviewer.jupyter.org/github/jocelyn-ong/data-science-notes/blob/master/content/04-basic-python/solutions/0404-python-functions-and-classes-solutions.ipynb){:target="_blank"}.
