---
layout: post
title:  "Commitment Issues"
date:   2016-08-02 17:06:25
categories: code
---

If you're British, you were probably first familiar with 'git' as an 'unpleasant or contemptible person'. Otherwise, you're probably familiar with Git as the version control system developed by Linus Torvalds, the creator of Linux. As much as I enjoy discussing British insults, I'm going to focus this blog post on the latter case.

Git is an incredibly helpful tool for developers, and most code newbies become familiar with the tool fairly early on. The basic git workflow is pretty straightforward:

* initialize a repository with <code>git init</code>
* add files with <code>git add</code>
* commit (take a snapshot of the current state of your files) with <code>git commit</code>
* push to your remote repository with <code>git push</code>

So far, relatively straightforward -- but Git is capable of so much more. I'm going to focus on local git commands, rather than anything using a remote repository (but check back for a future blog post on that!).

Branching Out
-------------

What if you want to create a new feature, but want to maintain your current code? Not a problem! Create a new branch in your repository.

Say I'm creating an app for an animal shelter. I have a barebones, but functioning app, which allows me to track the animals currently at the shelter. I can easily print out my first few commits using <code>git log</code>:

{% highlight ruby %}
git log
1c4709f Add Dog class (kallaugher, 4 seconds ago)
b472d69 Add Cat class (kallaugher, 27 seconds ago)
b5ae131 Initial commit (kallaugher, 49 seconds ago)
{% endhighlight %}

I'm ready to add a new feature to the app, showing the schedule for the employees of the shelter -- but I don't want to mess up the already existing functionality. Time to add a new branch!

{% highlight ruby %}
git checkout -b employee-schedule
git add employee.rb
git commit -m "create Employee class"
git add schedule.rb
git commit -m "create Schedule class"
git commit -am "add schedule routes to controller"
{% endhighlight %}

The <code>git checkout</code> command is key here. This confused me at first because I guessed (incorrectly) that it mean check out as in leave a branch -- the opposite is actually true. Think of it as checking out a book from a library when you're ready to read it for a while (shout out to [Vaidehi](http://vaidehi.weebly.com/) for that helpful metaphor).

What if I'm in the middle of working on this new feature, when I find out we're now accepting goldfish at the shelter, and I need to immediately add that functionality to the basic app? Not a problem! Just checkout the <code>master</code> branch (make sure to leave out the <code>-b</code> flag), edit and commit there, and then return to <code>employee-schedule</code> branch when you're ready:

{% highlight ruby %}
git checkout master
git add goldfish.rb
git commit -m "add goldfish to adoptable pets"
git checkout employee-schedule
{% endhighlight %}


When that feature is fully implemented, it's easy to merge the two branches into one. Just return to your <code>master</code> branch, merge in <code>employee-schedule</code>, and then delete it:

{% highlight ruby %}
git checkout master
git merge employee-schedule
git branch -d employee-schedule
{% endhighlight %}

Visualizing your branches
-------------------------

There are some great tools for visualizing your commit history ([GitX](http://gitx.frim.nl/) is one example). However, you can also get a pretty great depiction just from your terminal. Adding a few flags to your <code>git log</code> command will instantly illustrate your commit and merging history:

{% highlight ruby %}
git log --oneline --abbrev-commit --all --graph --decorate --color
*   8e18e76 (HEAD -> master) Merge branch 'employee-schedule'
|\  
| * 20808eb add schedule template
| * 20ac786 add schedule routes to controller
| * ff37af4 create Schedule class
| * 4136d24 create Employee class
* | c647697 modify Controller to add goldfish route
* | 00e26c5 add Goldfish class
|/  
* 8b4984e Add AdoptionController
* 1c4709f Add Dog class
* b472d69 Add Cat class
* b5ae131 Initial commit
{% endhighlight %}


You can see where the branches diverge and come together. This is a simple example, but this visual would be even more helpful for a more complex project with multiple branches.


If I could turn back time
-------------------------

What if you've made a terrible mistake in your last commit, and in the immortal words of Cher, need to turn back time? Not a problem! If it's an entire commit, you can use <code>git revert</code>:

{% highlight ruby %}
git revert 20808eb
[master 9ecbe97] Revert "add schedule template"
{% endhighlight %}


This will not delete your last commit, but will instead create a new commit without the previous changes. If you want no trace left, you can use <code>git reset</code> instead. You can also use <code>reset</code> to undo merges -- see [here](https://git-scm.com/blog/2010/03/02/undoing-merges.html) for more info.

Resources
---------

Here are some great resources I used while writing this post:

* [Think Like A Git](http://think-like-a-git.net/)
* [Git Magic](http://www-cs-students.stanford.edu/~blynn/gitmagic/)

*This is barely the tip of the iceberg when it comes to Git's functionality. I hope to dive in again in a future blog post, so leave me your favorite resources (and your preferred Git pun for the next title)!*
