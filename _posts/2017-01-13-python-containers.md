---
tags:
  - data-science
  - data-science-notes
  - python
---
Data Science Notes Series: Python Containers

{% include toc %}

# Recap: Python variables and basic operations

- Variable assignment
    - `=`
- Strings
    - Wrapped in quotes `''` or double quotes `""`
    - Get length of string: len("hello")
    - String slicing: "hello"[start_index:end_index]
        - Index starts at 0
    - Addition and multiplication works
- Numbers
    - Addition, subtraction, multiplication, integer division, float division, modulo, power
    - Integers: whole numbers
    - Floats: decimal numbers
- Booleans
    - True or False
    - Can be converted to numbers: (F = 0)
- Comparators
    - Greater than `>`
    - Greater than or equal to `>=`
    - Smaller than `<`
    - Smaller than or equal to `<=`
    - Equal to `==`
    - Not equal to `!=`

# Lists, Sets, Tuples, and Dictionaries

Python variables can be grouped together, and here are some of the ways you can do that.

## Lists

Lists are denoted by square brackets `[]`, and elements within a list are separated by commas.


```python
["this", "is", "a", "list"]
```




    ['this', 'is', 'a', 'list']



Lists can contain elements of different types.


```python
[1, "hello", 40, 10.5]
```




    [1, 'hello', 40, 10.5]



Addition works on lists.

- But only lists can be added to lists


```python
[1, 2] + ["hello", "world"]
```




    [1, 2, 'hello', 'world']



Multiplication works on lists.


```python
[1, 2, "hello", 10.5] * 3
```




    [1, 2, 'hello', 10.5, 1, 2, 'hello', 10.5, 1, 2, 'hello', 10.5]



Lists are mutable - elements in a list can be changed.

You can add elements to lists with the `.append(new_element)` method. This adds the element to the end of the list.


```python
my_list = [1, 2, 3, 4]
my_list.append("hello")
print (my_list)
```

    [1, 2, 3, 4, 'hello']


You can also do it with the `.insert(index, new_element)` method. This adds the element to the position index that you specified - remember that the index starts from 0.


```python
my_list.insert(0, "world")
print (my_list)
```

    ['world', 1, 2, 3, 4, 'hello']


You can get items from within a list using indexing (square brackets `[]`).


```python
my_list[0]
```




    'world'




```python
my_list[-1]
```




    'hello'




```python
my_list[2:]
```




    [2, 3, 4, 'hello']



You can remove items from a list. The `.pop()` method removes the last item from the list and returns/ outputs it.


```python
my_list.pop()
```




    'hello'




```python
my_list
```




    ['world', 1, 2, 3, 4]



`.remove(element_name)` removes the element that matches `element_name`. You can also run it without `element_name`, then it removes the first element in the list. If none of the elements match `element_name`, an error will be thrown.


```python
my_list.remove(3)
```


```python
my_list
```




    ['world', 1, 2, 4]



**NEW JUPYTER NOTEBOOK TIPS**

- Tab completion works in Jupyter notebooks on declared variables and valid functions/ methods
    - Type `my_` and hit `tab`, it should auto-complete and give you `my_list` (assuming you had already declared a variable called `my_list`
    - Type `my_list.ap` and hit `tab`, it should auto-complete and give you `my_list.append`
    - Type `my_list.` and hit `tab`, it will give you a list of valid methods for the variable `my_list`
- Jupyter notebook has a built-in help `function`
    - Access it by running `my_list.method?` (e.g. my_list.append?)
    - Or after typing my_list.append hold `shift` and hit `tab` twice for a floating window, or four times for a fixed-to-bottom-window

*Note: This works for all functions and methods, not just lists.*

Try accessing the help for the `.extend()` method.

Now, add the elements "twenty", 50, and 100.6 to the end of my_list using the `.extend()` method.


```python
# add "twenty", 50, 100.6 to the end of my_list

```

You can also create lists using the `list()` method.


```python
another_list = list()
another_list.append(1)
another_list.append("two")
another_list
```




    [1, 'two']



## Sets

Sets are like lists, but you can't have repeated elements. Sets are denoted by curly braces `{ }` and elements are separated by commas.


```python
# lists can have repeats
[1, 2, 3, 4, 1]
```




    [1, 2, 3, 4, 1]




```python
# but sets cannot
{1, 2, 3, 4, 1}
```




    {1, 2, 3, 4}



You can also create a list by calling `set()`.


```python
my_set = set()
```

Then adding to it by using the `.add(element)` method.


```python
my_set.add(2)
```


```python
my_set
```




    {2}



## Tuples

Tuples are denoted by parenthesis `()`. Tuples are immutable - you cannot change elements of a tuple.


```python
my_tuple = (1, "world", "python")
```


```python
type(my_tuple)
```




    tuple



If you type `my_tuple.` and hit `tab`, you won't see methods similar to `my_list` - you can't add to `my_tuple` or take things out of it. (You can still slice and index it.)


```python
my_tuple[1:]
```




    ('world', 'python')



## Dictionaries

Dictionaries are a little different when compared to lists, sets, and tuples. In another programming language, you might call it a hash map. In a dictionary, instead of elements indexed by position, you have key-value pairs (or values 'indexed' by key).

- i.e. To call a value in a dictionary, you use its key instead of its position
- Keys can be strings, integers, floats, or tuples
- Dictionaries are unordered, the order in which you put key-value pairs in may not be preserved
    - There is an `OrderedDict` in the `collections` module which can be imported - more on importing modules later on
- Dictionaries are denoted by curly braces `{}`
- You can also create dictionaries by calling `dict()`


```python
my_dict = {"test": 1, 12: "Hello World", (1,5): "python"}
```


```python
my_dict
```




    {(1, 5): 'python', 'test': 1, 12: 'Hello World'}




```python
my_dict[(1,5)]
```




    'python'




```python
my_dict['test']
```




    1



You can add to dictionaries as well with the square bracket notation.


```python
my_dict['new_key'] = "new_value"
my_dict
```




    {(1, 5): 'python', 'test': 1, 12: 'Hello World', 'new_key': 'new_value'}



`.pop(key)` will remove the key-value pair (where the keys match) in a dictionary and return/ output it. If the key provided doesn't match anything in the dictionary, a key error will be thrown.


```python
my_dict.pop("a")
```


    ---------------------------------------------------------------------------

    KeyError                                  Traceback (most recent call last)

    <ipython-input-37-cc63ecc48ee7> in <module>()
    ----> 1 my_dict.pop("a")


    KeyError: 'a'



```python
my_dict.pop("test")
```




    1




```python
my_dict
```




    {(1, 5): 'python', 12: 'Hello World', 'new_key': 'new_value'}



Some other useful dictionary methods:

- `.items()`: returns the key-value pairs in the dictionary as a list of tuples (key, value)
- `.keys()`: returns a list of keys in the dictionary
- `.values()`: returns a list of values in the dictionary


```python
my_dict.items()
```




    dict_items([((1, 5), 'python'), (12, 'Hello World'), ('new_key', 'new_value')])




```python
my_dict.keys()
```




    dict_keys([(1, 5), 12, 'new_key'])




```python
my_dict.values()
```




    dict_values(['python', 'Hello World', 'new_value'])



# Practice

- Create a list of numbers and assign it to a named variable
- Add another number to the middle of your list
- Sort your list in ascending order (hint: check out list methods using tab completion)
- Convert your list to a set (hint: remember that you can create sets using the `set()` method)


# Up next

I reorganized my Python notes a little, and we'll have three more notebooks on Python:

- Conditionals and loops
- Functions and classes
- External files and libraries

This notebook with solutions can be viewed [here](http://nbviewer.jupyter.org/github/jocelyn-ong/data-science-notes/blob/master/content/04-basic-python/solutions/0402-python-containers-solutions.ipynb).
