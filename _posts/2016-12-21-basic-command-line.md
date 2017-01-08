---
tags:
  - data-science
  - data-science-notes
  - command-line
---
Data Science Notes Series: Introduction to the command line

As I mentioned in [a previous post](https://jocelyn-ong.github.io/quick-check-in/){:target="_blank"}, I wanted to create a revision repository with easy-to-follow notes and also post short posts on tools and tutorials. I've decided to combine the two, and will be bringing you part of my notes as a series of posts, together with [the GitHub repository](https://github.com/jocelyn-ong/data-science-notes){:target="_blank"}.

{% include toc %}

# Basic command line

We used GitHub a lot and also did a little bit of our code work on the command line.

Sometimes, we ran into issues because we didn't have enough experience with the command line. So I thought it would be useful have a little command line practice.

# Open up Terminal

You should be able to open up Terminal by typing `⌘ (command) + space`, then `Terminal` into Spotlight. Or you can find it in `Applications/Utilities`. You should get something that looks like this:

[![terminal]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/terminal.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/terminal.png)

It may look a little different in terms of style and color, but you should see a prompt that says `user@computer-name:~$` followed by a cursor.

*If you're interested in styling your Terminal, check out [this link](http://osxdaily.com/2013/02/05/improve-terminal-appearance-mac-os-x/){:target="_blank"}.*

# Basic commands

## Navigation

You can access just about anything on your computer through the command line - you just need to know how to do it.

### `pwd`

`pwd` stands for `print working directory` - it tells you which folder you are currently in Terminal.

Syntax: `pwd`

[![terminal-pwd]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/terminal-pwd.png)]({{ site.url }}{{ site.baseurl }}/images/terminal-pwd.png)

The output is the path to the directory. It always starts with the main 'root' folder, which is `/Users/` for my case, followed by the next level folder, which for me is my username, and so on, until you reach the folder you're currently in.

This format is also known as the absolute path to the folder, and it is what `pwd` will always return. We'll learn about relative paths later on.

### `ls`

`ls` lists out the files and folders in your current directory

Syntax: `ls` or `ls -A` or `ls {PATH}`

[![terminal-ls]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/terminal-ls.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/terminal-ls.png)

If you have any hidden files (the ones starting with a .), you'll notice they didn't show up here. If you do want them to show up, use `ls -A`.

[![terminal-lsA]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/terminal-lsA.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/terminal-lsA.png)

You can also list the contents of another directory, by doing `ls path_to_directory`

[![terminal-lsdir]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/terminal-lsdir.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/terminal-lsdir.png)

*Remember: You can get path_to_directory by using `pwd` in the directory that you need the path for.*

You notice that instead of typing `ls /Users/joce/Movies`, I simply typed `ls Movies`. This is known as a relative path - you're point to a folder by its position relative to your current working directory. In this case, the folder `Movies` exists in my current working directory (which is `/Users/joce/`), so I can just call it without having to add `/Users/joce/` in front of Movies. Note that if I typed `ls /Users/joce/Movies/`, it would have returned the same output. More on relative paths later on.

### `cd`

`cd` stands for `change directory` - it allows to change your current working directory (the folder you're currently in)

Syntax: `cd {FOLDER/PATH}` or `cd` or `cd ..`

[![terminal-cd]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/terminal-cd.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/terminal-cd.png)

You'll notice that our prompt has changed to `user@computer-name:~/folder-name$` - you're now in another folder!

If you want to change directories to a folder that's not in your current working directory, specify the path to your destination folder.

`cd` without any additional parameters will change your current working directory to your home directory (i.e. /Users/username/).

`cd ..` changes your directory to the parent of your current working directory. For example, we're now in `/Users/joce/Movies`. If I ran `cd ..`, I'm going to end up in `Users/joce/`. This is another example of using a relative path.

## Path referencing

There are two ways we can reference paths - absolute and relative. We touched a little on relative paths above - `..` points to the parent directory, and child directories can be called directly without any other path prefixes.

Absolute paths are those where the path to the file is written out in full starting from your home directory. For example `/Users/joce/Movies/` is the absolute path to my `Movies` folder. Absolute paths are called that because they never change (unless you move the folder elsewhere).

In contrast, relative paths change depending on where you currently are. For example, if I'm currently in `/Users/joce/`, my relative path to `Movies` is just `Movies`. However, if I'm currently in `Users/joce/Desktop/Example/`, then my relative path to `Movies` will be `../../Movies`. Note that we use `..` twice to move up two levels to get to `/Users/joce/`.

## Creating/ deleting folders and files

### `mkdir`

`mkdir` stands for make directory, and it's used for creating folders.

Syntax: `mkdir {FOLDER/PATH}`

[![mkdir]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/mkdir.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/mkdir.png)

To create a folder in your current working directory, use `mkdir {FOLDER}`. To create a folder wherever in your computer, just include the path to wherever before the folder name!

### `touch`

`touch` is used to create files - any kind of file.

Syntax: `touch {FILE/PATH}`

[![touch]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/touch.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/touch.png)

Similar to `mkdir`, use `touch {FILE}` to create a file in your current working directory. Remember to include the file type extension! Include the path if you wish to create the file in another location.

### `rm`

`rm` is used to remove files or folders.

CAUTION: `rm` is irreversible. Be really careful about using it.

Syntax: `rm {FILE/FOLDER/PATH}`

[![rm_file]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/rm_file.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/rm_file.png)

[![rm_folder]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/rm_folder.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/rm_folder.png)

Note that when we ran `rm new_folder/`, the command line returned a message that told us `new_folder/` is a directory. Adding the flag `-r` tells the command line to run `rm` recursively - remove the named folder and all items inside it.

## Copying and moving files/ folders

### `cp`

`cp` is used for copying files/ folders

Syntax: `cp {SOURCE_FILE/PATH} {DESTINATON_FOLDER/PATH}` or `cp -r {SOURCE_FOLDER/PATH} {DESTINATION_FOLDER/PATH}`

[![cp_file]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/cp_file.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/cp_file.png)

[![cp_folder]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/cp_folder.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/cp_folder.png)

Like `rm`, `cp` can't be used on the folder directly, but you can use the flag `-r` to copy the contents of the folder, and paste it in another folder (which you can then name it exactly like the source).

### `mv`

`mv` is used for copying files/ folders

Syntax: `mv {SOURCE_FILE/PATH} {DESTINATON_FOLDER/PATH}` or `mv -r {SOURCE_FOLDER/PATH} {DESTINATION_FOLDER/PATH}`

[![mv_file]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/mv_file.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/mv_file.png)

[![mv_folder]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/mv_folder.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/mv_folder.png)

## `open`

Use `open` to open files using the default Application.

We can also use the command line to open Applications, or specify which Application to use when opening files.

# Tip: Tab completion

Tab completion works in the command line (it'll work elsewhere too, we'll get to that later). To use it, type the first few letters of a file or folder, then hit `tab` - if it's the only file or folder beginning with those letters, the command line will autocomplete it. If nothing happens, hit `tab` twice, and the command line will output all the files or folders starting with those letters (then you can add another letter - or as many as required to make it unique - before hitting `tab` again).

[![tab-completion]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/tab-completion.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/command-line/tab-completion.png)

Sometimes, you may have some files that have the same starting and different endings, e.g.:

- `this_is_file_one.txt`
- `this_is_file_two.txt`

In this case, if you typed `th` and hit `tab`, autocomplete will give you `this_is_file_` and wait for you to continue the rest of the file name (or hit `tab` twice to see the list of possible files).

# Until next time

Thanks for taking the time to read this and I hope that it's useful to you! Stay tuned to my next post in the series and follow me on [Twitter](https://twitter.com/joce_ong){:target="_blank"} where I share interesting data related posts!
