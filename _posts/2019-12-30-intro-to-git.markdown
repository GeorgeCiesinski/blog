---
layout: single
title:  "Introduction to Git"
date:   2019-12-30 23:00:00 -0500
categories: Git Github
comments: true
---

Hi everybody! I am excited to finally be able to make a programming related blog post on my new blog. In this post I will be talking about an extremely useful tool for any programmer: Git. 

This is my first blog post in the Git series which I have been planning for some time. I will be covering the "Introduction to Git" in a sort of "in a nutshell" format. Although I wont be diving heavily into the details, I will be linking some of my favorite resources for further reading. I recommend checking these resources out at the end of finishing this post. Without further adieu, please enjoy the rest of my post!

# What is Git?

Git is a version control system, or VCS. Think of it as an advanced saving tool. In other words, it's kind of like CTRL+S but each time you save, Git creates a new version so that your old versions do not disappear and are not overwritten. In fact, Git creates a timeline of all of all the versions your project went through since the very first version you ever saved. If you don't like the latest version, or if you need to revisit an older version, Git lets you load that version at any time.

For more information on what Git is, I recommend [this tutorial](https://www.atlassian.com/git/tutorials/what-is-git "What is Git") found on the [Atlassian Bitbucket website](https://www.atlassian.com/git/tutorials "Git Tutorials"). This site has many excellent tutorials that go much more in depth on all of the subjects that I cover.

# How does Git work? 

The subject of how Git works is actually pretty [complicated.](https://git-scm.com/doc "Git Documentation") The main thing to remember is that git lets you create a repository for your program, in which you can create many different commits and branches. I will explain each of these things below. 

I also wanted to mention that if you want a more thorough explanation of how Git works, Git provides a free e-book called [Pro Git](https://git-scm.com/book/en/v2 "Git Pro Book") which covers everything from the basics to Git Internals for the nerds out there. 

## What is a repository?

A repository can be thought of as a folder containing your project, in which Git tracks the project files. Every time you add a file to this folder and include it in a commit (this will be explained next) Git starts tracking the file. This means that any changes made to the file, whether code is added, removed, or if the file itself disappears, will notify Git, which will in turn notify you. Once you are notified, you can decide whether you want to commit (think "save") the changes, or not. Git lets you keep a remote repository, and one or more local repositories. 

A remote repository is kind of like a **central** folder all of your team members can see. If you are not working in a team, think of this as a cloud where you can backup your local repository. Any changes to the remote repository which are made by somebody else need to be pulled (downloaded) to your local repository for you to see them locally.

This brings us to the local repository. Think of this as the actual project folder on your computer. Each team member has their own local repository containing all of their changes. These changes remain completely private until they are pushed (uploaded) to the remote repository. 

![Remote Repository](/assets/images/Git/Remote-repository.png)

## What are commits?

Commits are snapshots of your program. In other words, they are the many different versions of your program that you can create. In practice, any small bit of progress you make on your project, whether it works or not, can be "committed" (think "saved"). These snapshots don't simply create a whole new copy of the project. Instead, they contain the changes from the last commit. They might list things like the lines of code you removed, the lines of code you added, and any new files that are not in the last commit. Each commit has a hash value, which is like a long alphanumeric string. This is used to identify the different commits. Optionally, you can also add a message to your commit which makes it easier for a human to tell what you added or changed in that specific commit. Below, you can see what a commit looks like on Github:

![Git Commit](/assets/images/Git/Commit.png)

## What are branches?

Branches can be thought of as different lines of development, each containing a chronological series of commits. A git repository starts with a single branch - typically called "master" or sometimes "develop". As the name "branch" implies, this branch can be branched out into multiple branches. This gives you the advantage of working on multiple parts of the program at once. 

For example, lets say you and your buddy want to develop a program at the same time. You want to build the user interface, while your buddy wants to code the back end to make it do things. If you both share the same branch, you risk repeatedly overwriting each others code. Instead, you can create a new branch for you called user\_interface and a new branch for your buddy called back\_end. While working in your user\_interface branch, you can create multiple commits without affecting your buddy's code. Once you are finished, you create what is called a merge commit, which combines your branch back into the original branch you branched off from. Once your buddy is done, he can do the same. The main advantage of branches is that commits within that branch do not affect any other branch. 

Below is an example of what branches look like when visualized on Github. The three branches are Master, Develop, and gh-pages in this example: 
![Branches](/assets/images/Git/Branches.png)

There is one **important** thing to note when working with branches. Although you and your buddy can work on the same file if you are in two separate branches, Git will probably raise a merge conflict when both of you merge your work back into the original branch. This is because Git will get two conflicting changes to the same file, and it will need your intervention to specify whether to keep your changes, keep your buddies changes, or to manually rewrite the file to resolve the conflict. Because of this, best practice is to avoid working on the same script or file at the same time, even if you are in different branches. As long as you don't try to modify the same file, you should never encounter a merge conflict.

## What are Git Hosting Sites?

Earlier, I discussed remote repositories and how they are similar to cloud backups of your code, but I never explained where these are hosted. There are lots of options for hosting Git repositories out there. Many different companies have popped up including Github and Bitbucket which let you create an account and create your own repositories. They have many different free and paid plans that let you collaborate with a number of others. You can find a larger list of these [here.](https://git.wiki.kernel.org/index.php/GitHosting "Public Git Hosting Sites") 

You can also host your own git server, although I will not be getting into that. If you are interested in this option, there are many tutorials available online which can be found through a Google search. 



# Why do I need Git?

There are many great answers for this question. In order to answer this, let's consider your other options.

## Alternative #1: Forget about version control. CTRL+S is good enough for me!

So you decided to go old school - and by old school, I mean what you have used since you got your first computer. Ever since the first essay your teacher let you write in Microsoft Word, you lived by the adage "Jesus saves". The wise would always back up their work by clicking File and then Save, or CTRL+S. Why should programming be any different? 

I'll tell you why... from experience. This method maintains a single working version of your program. Every time you add code and save the project, it overwrites the old version. Let's say you have a working smart home app which can control the lights in every room of your house, but you want to add a new feature arming the automated defense turrets when you leave for work. A feature like this can take many lines of code and should be saved numerously to prevent any loss of progress. If your smart home app stops working properly while you are trying to code this new feature, then your only two options are:

1) Figure out what you broke, and fix it.
2) Mash CTRL+Z to undo all of your work until the program works again.

Both of these options are extremely time consuming and have the potential to cause more damage. Let's consider the next alternative. Perhaps you were successful with your change, but realize that you like the old version... Well, now you have to rewrite the program back to how it was before. You can't simply switch back to the older version.

## Alternative #2: Manually maintain multiple saves of the program, of course!

You decided to apply your Galaxy brain. One version is obviously too few. You decided to save your program as a new version every single time you add a feature. It doesn't matter if you maintain the versions on a cloud, or in separate folders. 

This system has some obvious advantages and disadvantages. On one hand, you now can save your version, and start a new version where you tackle the automated defense turrets feature. If you break the project, getting back to a working version is as simple as navigating to the folder containing this version. 

Before long, you find that you have dozens of folders containing the same project. Even if you are proactive and come up with a naming convention to keep track of the different versions, such a system is time consuming and still poses challenges when it comes to reverting to a specific version of the project. Luckily, it doesn't really matter too much because you quickly find yourself running out of hard drive space from all of the different copies of the same code you have. Jokes aside, it is incredibly inefficient to duplicate your project dozens of times to maintain multiple working versions.

## Using Git

Git gives you the advantages of Alternative #2, but without the disadvantages. The ability to save snapshots instead of multiple folders containing the complete project is the enormous size difference. A snapshot is simply metadata that essentially says "99 of the files are unchanged. One file has the following lines of code removed, and contains the following new lines." Every new snapshot might contain bytes or kilobytes of new data, while saving each version of your project in a new folder multiplies the entire project size.

It is also easier to find and revert to older versions of your project. Instead of sifting through multiple folders, Git lets you organize your commits into different branches, and lets you summarize each commit with a message telling you exactly what you changed. It also lets you see the metadata so you know exactly which lines changed, and which files were added or removed. You can decide to switch between branches or switch to a specific commit, depending on what you need.

These are all very powerful tools which can make your life much easier, especially when a bug is affecting a new version of your program but not an old one, when you accidentally break your program while coding a new feature, or when you want to collaborate with a team without the headache of sharing the same code. 

In order to appreciate the simplicity and efficiency of Git, I highly recommend trying it for yourself. In my next blog post, I will cover how to initialize a git repository, and how to use the Git command line. 

