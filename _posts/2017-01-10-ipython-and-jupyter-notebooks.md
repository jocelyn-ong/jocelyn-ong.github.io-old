---
tags:
  - data-science
  - data-science-notes
---
Data Science Notes Series: IPython and Jupyter Notebooks

{% include toc %}

# IPython and Jupyter Notebooks

Jupyter notebooks are a great way to run, test, and share code. It is organized by cells, and the cells can by run one by one. So if you have a piece of code that might be made up of many different independently executable portions, you could split them up and run them piece by piece to make sure everything works. (Of course, if everything is just one big function, then we may not be able to do that.)

# Installation (including Anaconda)

Before we install IPython and Jupyter notebooks, we'll need to install some `helpers`. These will help us with the installation of IPython, Jupyter notebooks, Python and its various related libraries later on.

- Head over to the [Anaconda website](https://docs.continuum.io/anaconda/install){:target="_blank"} and follow the instructions to install Anaconda.
    - You'll see that there are 2 separate installers for Python 2 and Python 3. Pick Python 3, if we need Python 2 later one, we can still install it.
    - Keep the default options.

Now in terminal, try running `jupyter-notebook` in terminal (preferably in your root user folder) - if it was successfully installed it, a new tab or window should pop up in your default web browser. Otherwise, check out the [documentation](http://jupyter.readthedocs.io/en/latest/install.html){:target="_blank"} for troubleshooting.
- `pip` should also have been installed if the above was done correctly. Test it by running `pip install -U pip` in terminal. Otherwise, check out the [pip documentation](https://pip.pypa.io/en/stable/installing/#upgrading-pip){:target="_blank"} for troubleshooting.

The tab that pops up should look pretty much like a folder view. The reason for booting Jupyter notebook is that you will be able to access all child folders of the folder you're currently in, but you cannot access its parents via the browser.

# Code, Markdown, and Raw

Let's create a new notebook. In the top right of your tab, you should see a button called `new`. Click on it and select the option under `Notebooks`.

[![folder_toolbar]({{ site.url }}{{ site.baseurl }}/images/ds-notes/jn/folder_toolbar.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/jn/folder_toolbar.png)

## Code

A new tab should open with your new untitled notebook, with a single empty cell.
- By default, the cell is formatted to be a `code` cell. That means that what ever you type into it has to be a piece of valid code (Python code in our case) for Jupyter notebook to run it successfully.

## Markdown

Select the cell (so that you see your blinking cursor in it), and look for the `Cell` button in your toolbar. Under it, you should see `Cell Type`, then select `Markdown`.
- `Markdown` is a text format. Read more about it [here](https://daringfireball.net/projects/markdown/){:target="_blank"}.
- Now you can type any text into the cell, and no error will be thrown when you run it.

To run a cell, select it and press `shift + return`.

## Raw

The last cell type is `Raw`. If you select `Raw` as a cell type, it means nothing happens when you try to run the cell, no code, no text formatting, nothing.

It's useful for when you want to retain a piece of code in your notebook but you don't want it to run.

# Navigation shortcuts

Everything in a Jupyter notebook should be a accessible via point-and-click, but sometimes, it's much easier to work with keyboard shortcuts. Here are some of my most used ones:
- `shift + return`: Run cell
- `return`: Enter edit mode on selected cell
- `Esc`: Exit edit mode on selected cell

For the following shortcuts to work, you must not be in edit mode
- `a`: Add one cell above selected cell
- `b`: Add one cell below selected cell
- `s`: Save notebook
- `c`: Copy cell
- `x`: Cut cell
- `shift + v`: Paste copied cell above selected cell
- `v`: Paste copied cell below selected cell
- `shift + m`: Merge selected cell with cell below it, resulting cell type follows the current selected cell
- `m`: Change cell type to markdown
- `y`: Change cell type to code
- `r`: Change cell type to raw
- numbers `1` to `3`: Change cell type to markdown and make them headings 1, headings 2 or headings 3

For a comprehensive list of keyboard shortcuts, exit edit mode and hit `h`.

# nbextensions

I found some useful add-ons to Jupyter notebook in the [nbextensions](https://github.com/ipython-contrib/jupyter_contrib_nbextensions){:target="_blank"} which requires a separate installation. It's not essential, so if you would like to install it, head over to [this link](https://github.com/ipython-contrib/jupyter_contrib_nbextensions){:target="_blank"} and follow the instructions. Otherwise, we're good to go!

# Up next

Now that we've got everything set up (well almost everything), we can get on to Python. Why Python? Because it's a popular language for data science and I found it easier to learn than R. In my future posts of this data science notes series, it will be helpful to have a copy of my Jupyter notebook so that we can run the code together, and you can have some practice writing code and running it.

Thanks for taking the time to read this and I hope that it was useful to you! Stay tuned to my next post in the series and follow me on [Twitter](https://twitter.com/joce_ong){:target="_blank"} where I share interesting data related posts!
