---
tags:
  - data-science
  - data-science-notes
  - github
  - git
---
Data Science Notes Series: More GitHub

{% include toc %}

# More GitHub

With public repos on GitHub, we have access to a lot code hosted by other people. Sometimes, the code is for a tool that you're using, and you have this great idea for improving it. Or maybe it's just a good piece of code that you would like to use - e.g. [Jekyll templates for GitHub Pages](https://github.com/jekyll/jekyll/wiki/Themes){:target="_blank"} (I use this!). Or maybe you're attending a data science course and you want to make a copy of your instructor's repo for your own use. Forking makes it easy for us to do all that.

What if you're working on your own code and you wanted to test something out without destroying what you've already built? Branches can help you with that.

And if you've already screwed up and need to turn back time? `Reset` saved me a few times on this.

# Forking

Forking allows you to make a copy of someone else's repo. To do so, simply head to the repo that you want to copy, then click on the button that says `Fork` on the top right hand corner of the browser.

[![forking]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/forking.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/forking.png)

You might be asked where you want to save it to, select your username and proceed. The page should then redirect to your newly created copy of the repo.

# Branching and Checkout

## Branches

Branches allow you to work in different directions on the same project - useful if you want to test out different ideas. When a repo is created, there is just one default branch - `master`. To create a new branch, click on the button that says `Branch` and name your new branch.

[![create_branch]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/create_branch.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/create_branch.png)

There's another way to create branches - via terminal. We'll see how to do that in a bit.

[![default_branch]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/default_branch.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/default_branch.png)

You can also change which branch is your default. You should see the above toolbar in the main page of your repo -  click on `branches`, and that should bring you to a page where you can change your default branch.

[![default_branch_2]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/default_branch_2.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/default_branch_2.png)

To learn more about branches, check out the [documentation](https://git-scm.com/docs/git-branch){:target="_blank"}.

## `git checkout`

To work with your different branches, you'll need to use `git checkout`. `git checkout` can also be used to work with a previous version of your work.

If you remember, we update our GitHub repo by committing our local changes to the server - there's a record of all the commits that we made previously. You can view all of the commits in the same toolbar that we used above, by clicking on `commits`. Each commit has a 'link', which is what we'll use to tell GitHub which point we want to revert to.

- Note: your local copy gets written over when you run `git checkout`
- Running `git checkout LINK` in terminal (in a valid GitHub-linked folder) will revert all files to the state in that commit
    - To revert just one file, run `git checkout LINK FILENAME`

To be honest, the few times I used branching and `git checkout`, was when my code broke and I was trying to figure out how to get back to a point where it was working. It's really useful to know, and a little confusing to understand sometimes.

After you've checked out a previous commit, any commits you make after that don't follow your original path. Your commit history is now split in two. Now you can use your new path to create a new branch by running `git branch BRANCHNAME` after committing.

To learn more about checking out, take a look at the [documentation](https://www.git-scm.com/docs/git-checkout){:target="_blank"}.

# Reset

My most used `git` command on this page - `git reset`.

The way Git explains it, when you're working in Git, there are three work spaces - your working directory (local), your staging area (where updates are added), and the server HEAD (where updates are committed and pushed).

`git reset` works by undoing `git add` and `git commit`.

There are three types of resets:

- `git reset hard HEAD~`: `hard` means undo changes in your local, staging, and server. i.e. your local file will be overwritten! We're usually advised not to do this.
- `git reset soft HEAD~`: `soft` means undo changes only to the server. Your local file is unchanged, and any files in the staging area are also unaffected.
- `git reset mixed HEAD~`: `mixed` means undo changes only in the staging area and the server, your local file is unchanged. This is also the default, so if you run `git reset HEAD~` without the word `mixed`, it is assumed that the reset mode is `mixed`

Each time you run `git reset`, it moves back one commit. You can also specify the commit that you want to move back to with the commit link that we used above in `checkout`. Note that resetting removes all future commits - so if you want to save them, consider using `checkout` instead.

**Example use case**  

This happened to me several times. GitHub has a limit on the file size that can be hosted, but it doesn't warn you when you're adding and committing files that are too large. An error is only thrown when you try to push the changes to the server.

When that first happened to me, I thought 'Okay, I'll just remove that file from my folder, and re-add and re-commit everything.' Doesn't work like that - because that file still exists in the staging area! To remove it from the staging area, run `git reset HEAD~` until the point before you added those large files - you can check by using `git status` or checking your commit history.

Once that is done, remove that troublemaker file from your GitHub-linked folder and run `git add` and `git commit` again. Now, pushing to the server should no longer give you any problems.

If you want to learn more about `git reset`, the [Git blog has a great post on the topic](https://git-scm.com/blog){:target="_blank"}.

# Up next

And that should be enough for you to get started with Git and GitHub! In our next post, we'll learn about IPython and Jupyter notebooks - the notebook will be where we'll be doing most of our code work, then we'll be ready to move on to some basic Python!

Thanks for taking the time to read this and I hope that it was useful to you! Stay tuned to my next post in the series and follow me on [Twitter](https://twitter.com/joce_ong){:target="_blank"} where I share interesting data related posts!
