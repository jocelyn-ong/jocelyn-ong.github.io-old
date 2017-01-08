---
tags:
  - data-science
  - data-science-notes
  - command-line
---
Data Science Notes Series: A little more command line

{% include toc %}

# A little more command line

Welcome to the next post in the data science notes series. We previously covered the most basic commands on the command line - with them, we have enough tools to navigate around our file system. Here we cover some extra tools - good to know commands - and hopefully they'll help you in your workflow.

## `curl`

`curl` is a tool used to work with URLs - I use it mainly for downloading files from the Internet (especially text files).

Syntax: `curl -O {URL}` or `curl {URL} -o {FILE/PATH}` or `curl {URL}`

Note: The flags are capital letter O and small letter o, not zero.

[![terminal-curl]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/terminal-curl.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/terminal-curl.png)

I generally use `curl` with the flags `-o` or `-O`, because I want to save the data on the page into a file. `curl` without any flags would output the data on the command line.

Use `-o` if you want to specify the file name it is to be saved as or the path to where it should be saved. Use `-O` to save it to the current folder with the default file name.

## `echo`

`echo` is like printing - it tells the command line to print out a line of output.

Syntax: `echo text to be printed`

[![terminal-echo]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/terminal-echo.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/terminal-echo.png)

Note that `echo` doesn't work on files. For example, if you had a file called `a.txt` and tried `echo a.txt`, the only thing that would get printed out would be `a.txt` instead of the contents of the file. To print out the contents of a file, see the next section.

## `cat` or `less`

`cat` and `less` can be used to print out the contents of files to the terminal. The difference between the two are how many files you can print to the terminal and how the content is displayed.

Syntax: `cat file_one file_two file_three` or `less file_one`

[![terminal-cat]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/terminal-cat.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/terminal-cat.png)

With `cat`, you can print out one or more files at the same time, and the contents will be printed in the order that the file names are listed.

[![terminal-less]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/terminal-less.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/terminal-less.png)

With `less`, only one file can be printed, and it doesn't print it directly to the terminal. Instead, it is as if a separate pane is opened up to show the contents, and only the content that fits into your current window will be showed. To continue showing more content, hit `return`. To end the preview, hit `q`.

## `>>`

`>>` allows you to write to a file (I usually use it only for text) and can be used in conjunction with echo (or other commands).

Syntax: `a >> file_name` where `a` is a script/ command, and its output will be printed to the file.

[![terminal-output]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/terminal-output.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/terminal-output.png)

In the above example, the whole line `echo this is a test` replaces `a`, and the text `this is a test` is printed to the file called `d.txt`.

## Aliases

I like command line aliases - I use them for commands that I regularly type, or for applications that I frequently use (in conjunction with data science). Aliases are not really commands, and you don't assign them within the command line - they are created in a 'text' file called .bash_profile.

Syntax: `alias {SHORTCUT}="actual_command_line_command"`

To create an alias, we'll need to open up our bash profile. In the command line, type `open ~/.bash_profile`. This should open up the file in your default text editor. Add the alias to the document - for example, if I wanted a shortcut for `mkdir my_folder` called `mmf`, i would add `alias mmf="mkdir my_folder"`. Save and close the file, and back in the command line, type `source ~/.bash_profile` (this 'relinks' the file so that your commands and aliases are updated).

Now every time I run `mmf` in the command line, a folder called `my_folder` will be created in my current working directory.

# More command line practice

Knowing just the basic commands should be sufficient for now, but if you want to learn more, try out the [command line course on Codecademy](https://www.codecademy.com/learn/learn-the-command-line).

More commands will also be introduced in other posts as having the context should make those easier to learn.

# Until next time

Thanks for taking the time to read this and I hope that it's useful to you! Stay tuned to my next post in the series and follow me on [Twitter](https://twitter.com/joce_ong){:target="_blank"} where I share interesting data related posts!
