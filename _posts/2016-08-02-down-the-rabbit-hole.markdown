---
layout: post
title:  "Down the Rabbit Hole"
date:   2016-08-02 17:06:25
categories: code
---

<img src="/images/white-rabbit.jpg" class="right" caption="Down the debugging rabbit hole" />

Error messages are a little intimidating as a code newbie, but an important step is learning to embrace them. I’m learning with each error, and there’s nothing like the feeling of successfully debugging your code.

I ran across an interesting error yesterday, and I thought I would begin my inaugural blog post by digging into it. I was writing a simple program that require input from a user, using gets.

#gets == simple ?
-----------------

At first glance, <code>#gets</code> seems to be a very straightforward method, allowing you to save something that a user inputs at the command line.  It was one of the very first Ruby methods that I learned. I thought it was pretty cool — it immediately adds interactivity, and made me feel like I was actually creating something dynamic.

Let’s give a simple example:

{% highlight ruby %}
puts "Hello, what is your name?"
name = gets.chomp
puts "Hello #{name}!"
{% endhighlight %}

I’ll be asked for my name, enter it, and the terminal will output “Hello Alice!”

So far, so good. Easy, right? I didn’t really give much thought to what was going on under the hood until that strange error yesterday:

{% highlight ruby %}
jukebox.rb:29:in 'gets': No such file or directory
@ rb_sysopen ---format (Errno::ENOENT)
{% endhighlight %}


Clearly there is something strange going on with gets here, but beyond that I don’t have much of a clue. A few minutes googling and experimenting and I came up with a couple of answers on stackoverflow and ruby-forum.com. A couple of solutions were offered:

+ using <code>$stdin.gets</code> instead of <code>gets</code>
+ adding ARGV.clear on the line before <code>gets.chomp</code>

Both of those worked for me, and my code was running smoothly! It’s pretty tempting at this point to give myself an internal high five and move on, but I’m going to try and dig in a little further.

A closer look at #gets
----------------------

What happens when you call <code>#gets</code>? If you search Ruby Docs, you’ll find two separate methods, one from the <code>Kernel</code> class, and one from the <code>IO</code> class. Let’s have a look at excerpts of the two definitions:

><code>Kernel::gets</code>
>
>Returns (and assigns to $_) the next line from the list of files in ARGV (or $*), or from standard input if no files are present on the command >line.

><code>IO::gets</code>
>
>Reads the next “line” from the I/O stream

<code>ARGV</code> sounds familiar from the second solution I found -- now what is it? <code>ARGV</code> is a constant that stores the values input to a program via the command line. (For more information, see Joe O'Conor's [helpful blog post](http://jnoconor.github.io/blog/2013/10/13/a-short-explanation-of-argv/).) If it is empty, <code>Kernel::gets</code> will read from the standard input.

My best guess is that since my error disappears if I use <code>ARGV.clear</code> before <code>gets.chomp</code>, the issue lies in some value formerly input to the command line remaining in <code>ARGV</code>, and preventing <code>gets</code> from automatically returning the next line from the standard input. I am therefore using the first <code>gets</code>, from the <code>Kernel</code> class.

The <code>#gets</code> method that I want always read from the I/O stream (taking in the input from the user). Therefore, I can eliminate this error, by using <code>IO::gets</code> instead of <code>Kernel::gets</code>. I can specify this by using <code>$stdin.gets</code> (just as my original googling revealed!).

*I hope you've enjoyed my trip down the debugging rabbit hole! if you ever encountered this problem before, or have any information to share about <code>gets</code>, be sure to post a comment below!*
