---
tags:
  - projects
  - data-science
  - data-science-notes
  - github
  - git
---
Data Science Notes Series: Introduction to Git and GitHub

{% include toc %}

# Introduction to Git and GitHub

## What is Git

- Version control software
- Open source
- Developed by Linus Torvalds (creator of the Linux operating system kernel)
- https://git-scm.com{:target="_blank"}

## What is GitHub

> GitHub is a code hosting platform for version control and collaboration. It lets you and others work together on projects from anywhere.  
*Source: https://guides.github.com/activities/hello-world/{:target="_blank"}*

- GitHub uses Git for version control

## What is version control

- Records changes to files over time
- Versions are saved as snapshots
    - Which can be retrieved at a later time if required

# Why we're learning about Git and GitHub

- Version control might be useful when we're coding
    - If we screw up somewhere, hopefully we have a snapshot of a previous version that we can revert to and resume working from that point
- Open source
    - If you keep your GitHub repo public, others will have access to your code - it's a great way to share code with the community
        - My data science notes series is hosted on [my GitHub repo](https://github.com/jocelyn-ong/data-science-notes){:target="_blank"}

# Setting up Git and GitHub

## Install Git

- First, check if Git is already installed
    - Run `git --version` in the command line
- If Git is not installed, running `git` in the command line (on Mac OS X Mavericks 10.9 and above) will prompt you to install it
    - Read more about installation [here](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git){:target="_blank"}

## Set up Git

- Run `git config --global user.name "YOUR NAME"` in the command line
    - Replace "YOUR NAME" with your username
    - This is the username that will be associated with your Git commits
- Run `git config --global user.email "YOUR EMAIL ADDRESS"` in the command line
    - Replace "YOUR EMAIL ADDRESS" with your email address
    - This is the email address that will be associated with your Git commits

## Create a GitHub account

- Go to https://github.com{:target="_blank"} and create an account
    - The personal/ free account should be sufficient

## Set up GitHub authentication

- We'll be linking files between GitHub and our computers and to do that, we'll need some form of authentication
- [HTTPS](https://help.github.com/articles/caching-your-github-password-in-git/){:target="_blank"}
    - Run `git credential-osxkeychain` in the command line to see if the keychain helper is already installed
        - If it's not installed, you'll be prompted to install it
    - Run `git config --global credential.helper osxkeychain`
    - When you try to link a folder to GitHub, you'll be prompted for your GitHub password
        - This should just happen the first time
    - Read more about caching your password [here](https://help.github.com/articles/caching-your-github-password-in-git/){:target="_blank"}
- SSH
    - Another way to link up your folders is with SSH
    - Follow the steps [here](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/){:target="_blank"} to generate a SSH key

# Creating your first repository

- In GitHub, you should see a `+` on the top right corner of your page (right next to your profile icon)
- Click on it and select `new repository`
    - This creates a new 'folder' in your GitHub account
- You should be brought to a page that looks like this  
    [![git_first_repo]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/git_first_repo.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/git_first_repo.png)
- Go ahead and name your repository
- If you chose the personal/ free plan, you should only be able to create a public repository
- Also check the box that says `Initialize this repository with a README`
    - This creates a new markdown file in your new repository
        - Markdown refers to a file type
            - More on markdown files later!
- Below that box, you'll see a button that says `Add .gitignore None`
    - Ignore that for now, but a `.gitignore` file will be useful, so we'll come back to it soon!

# Cloning your first repository

- After you create your first repo, the page should automatically redirect you to the repo page
- You should see a button on the right that says `Clone or download`
- Clicking on it should give you something that looks like a link
    - You can choose to clone via SSH  
        [![git_clone_ssh]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/git_clone_ssh.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/git_clone_ssh.png)
    - Or HTTPS  
        [![git_clone_https]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/git_clone_https.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/git_clone_https.png)
    - To change the method, just click on the top right of the little box that pops out
- Copy the link by clicking on the little clipboard icon next to the link
- In terminal, `cd` to a convenient location
    - Maybe `Desktop`?
- Run `git clone LINK`
    - Replace `LINK` with the link that you copied  
        [![terminal_git_clone]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/terminal_git_clone.png)]({{ site.url }}{{ site.baseurl }}/images/ds-notes/github/terminal_git_clone.png)
    - If you chose to clone via HTTPS, and if this is your first time, you will be asked to key in your GitHub username and password
- And that's it, you've cloned your first repo to your computer!

# Up next

In my next post(s), we'll look at some common and basic commands that will allow you to update and retrieve information to and from your GitHub repo and your computer.

Thanks for taking the time to read this and I hope that it's useful to you! Stay tuned to my next post in the series and follow me on [Twitter](https://twitter.com/joce_ong){:target="_blank"} where I share interesting data related posts!
