---
tags:
  - projects
  - data-science
  - data-science-notes
  - github
  - git
---
Data Science Notes Series: Basic GitHub commands

{% include toc %}

# Basic GitHub commands

This post covers basic commands that allows interactions between files and folders on your computer with those on GitHub.

# Interaction between your computer and the server

`git` commands all begin with `git` in terminal. For and `git` command to work, the folder on your computer must be recognized as a valid GitHub folder.
- A valid GitHub folder has a `.git/` folder within it  
- If you run any command that begins with `git` in an invalid GitHub folder, terminal will return `fatal: Not a git repository (or any of the parent directories): .git`

If a folder was previously created using `git clone LINK`, the `.git/` folder would have been automatically created.

[![git_folder]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/git_folder.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/git_folder.png)

If you already had a folder on your computer that you now want to link to a GitHub repo, that `.git/` folder will have to be created. We'll look that how to do that in a bit.  

Recall that GitHub is a platform for version control, so when I say interaction, I mean files getting updated, changes being recorded, changes being flagged etc. For example, I have a file on my computer which I am working on. After I make a change, I want that change to be recorded, so I'll add the changes to the related GitHub repo and update it, so that the GitHub repo is now in sync with my computer.

# Init and Remote

## `git init`

Going back to a point above, to get your computer to interact with GitHub, we first have to have a `.git/` folder. This is called initializing.
- To do this, first `cd` into the folder which you want to host on GitHub.
- Next, run `git init` in terminal.

[![git_init]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/git_init.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/git_init.png)

And that's it, this folder will now be recognized by GitHub. But it's still not ready to interact with GitHub - because you can have several repos on GitHub, GitHub needs to know which repo you would want it to be linked to.

## `git remote`

And that brings us to the `git remote` range of commands. Remember the links you copied previously to run `git clone LINK`? `git remote` works with those links as well.

- `git remote -v` will tell you what GitHub repos your current folder is linked to, and how those links are named
    - `-v` for verbose  
        [![git_remote_v]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/git_remote_v.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/git_remote_v.png)  
        [![git_remote_v_none]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/git_remote_v_none.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/git_remote_v_none.png)  
    - Note that there was not output when I did that for `test-repo` - that's because we haven't linked it to anything yet

- `git remote add origin LINK` will tell GitHub which repo to link to your current folder
    - `origin` refers to the name of the link
        - `origin` is a default name - if a folder is created with `git clone LINK`, the name of the `LINK` will be `origin`
        - common names include `origin` and `upstream` (but you could really call them anything you like)
            - e.g. `git remote add upstream LINK`
    - I'll use my `training-repo` as an example  
        [![git_remote_add]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/git_remote_add.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/git_remote_add.png)
    - Caution: the command does not check if the `LINK` provided is valid, so ensure that you've copied and pasted it correctly.

What if I decided to change it to another repo? Or what if I change my repo name?

- `git remote set-url origin LINK` will tell GitHub to update the repo link of the current folder  
    [![git_remote_set_url]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/git_remote_set_url.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/git_remote_set_url.png)

You can also remove a GitHub link from your folder. For example, if you have both an `origin` and an `upstream` and you decide that you don't need one of them.

- `git remote rm upstream` will remove the `upstream` link
    - Replace `upstream` with the name of the link that you want to remove  
    [![git_remote_rm]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/git_remote_rm.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/git_remote_rm.png)

These are the four main `git remote` commands that I use all the time. To view the documentation, run `git help remote` in terminal (press `q` to exit the documentation) or [see it on the Git website](https://git-scm.com/docs/git-remote).

# .gitignore

Recall the `.gitignore` file we mentioned previously. This file will contain the list of files or folders that you want GitHub to ignore when you run `git` commands. You can create one when you create your repo on GitHub, but I usually just create one manually afterwards.

- In your folder in terminal, run `touch .gitignore`
- Then open it with your default text editor
- The file should be a list of file names or folder names
- Generally, the first thing I add to it is `**/.DS_Store`
    - `**` is a wild card
    - `**/.DS_Store` means any file called `.DS_Store` in any folder within the current folder
    - Add this, and any other files you don't want to host on GitHub, then save and close the file

# Pull, Status, Add, Commit, and Push

Now that we've set up the links between our folder and our GitHub repo, we'll be able to run several other commands.

## `git pull origin master`

`git pull` tells your computer to take the GitHub repo and update your local folder. It needs you to tell it which link to look at (if you have multiple links - e.g. `origin`, `upstream`) and which branch to look at. For now, we'll generally be working on the `master` branch, so the last term in the command will just be `master.

[![git_pull]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/git_pull.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/git_pull.png)

- In our first `git pull`, because there are differences between our test-repo folder and our training-repo on GitHub (remember the README.md file that we created?), the command will then take all the files in the repo on GitHub, and copy them to our local folder.
- If you run `git pull` again immediately, you'll see that your local folder is already up to date.

## `git status`

If you run `git status` in terminal now, you'll see that the `.gitignore` file that we created earlier is now marked as untracked. This means that GitHub doesn't know that it's there yet. Other statuses include:

- up to date
- changes to be commited
- branch is ahead of 'origin/master' by X commits

We'll see how the status changes as we run the next few commands.

## `git add`

To tell GitHub to track a file, we'll run `git add FILENAME`.
- If you have several files to be tracked, try using the wildcard `*` or use `.` to add all untracked files

[![git_add]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/git_add.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/git_add.png)

See that the status has now changed.

## `git commit -m "Commit message"`

`git commit` tells GitHub to save a snapshot of all the changes that are being tracked. GitHub requires a commit message for every commit, which can be added with the flag `-m` followed by the text.

[![git_commit]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/git_commit.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/git_commit.png)

In this case, note that the status has changed to "nothing to commit".

## `git push origin master`

If you go to your repo on GitHub now, you'll see that nothing has changed. We created a new file on our computer, told GitHub to track and commit the changes, but we don't see it on GitHub!

There's one final step to that - pushing.

Run `git push origin master` in terminal within your folder. Like `git pull`, it needs the link and the branch names to run.

[![git_push]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/git_push.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/git_push.png)

If you refresh your GitHub page, you should see the `.gitignore` file there now!

# Up next

Now we know how to track changes and files on GitHub - what if we screw up and want to go back to a previous version? Or what if we were tracking a file that we don't want to track anymore? I'll cover a couple more `git` commands in the next post - they really saved me a ton of headaches when things went wrong. We'll also take a look at forking and branches.

Thanks for taking the time to read this and I hope that it's useful to you! Stay tuned to my next post in the series and follow me on [Twitter](https://twitter.com/joce_ong){:target="_blank"} where I share interesting data related posts!
