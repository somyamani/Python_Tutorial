---
title: A note on programming languages, Ease Vs Speed
status: publish
layout: post
type: post
tags: [Python, Programming Languages]
categories: [Notes, Information]
author: Dilawar Singh
comments: true
---
 	
There are many ways one can think about programming languages. Its easy to get
lost in details.

For a newcomer, it is useful to keep in mind that languages are designed in
(more or less) for two purposes: first, to honor an mathematical abstraction;
and second, to be efficient in certain situations. The first category have only
few languages: Haskell, Lisp/Scheme and Prolog come to the mind. Rest of them
can be divided further depending on where they shine most. It is not enough
these days to ask which Programming language to use; one also have to say what
one wants to do with it.

C remains close to hardware, it can control your computer to great extent. This
is the fastest programming language (almost all the time). C/C++ rules on
embedded systems, simulators and kernels of your operating system. If you want
speed, you want C or C++. Cost: too much typing and taking care of low level
details.

Python, Perl and Java are higher-level. A great many details which C/C++
programmers must know are taken care by these languages behind the scene. The
cost is significant reduction in speed. Slower still are Matlab/Scilab and
Octave.

The rule of thumb: if it is easier to program in  a language, it is slower. Or
in other words, if you don't sweat is out, your computer has to sweat it out for
you. But who cares if computer sweat?

### Why Python?

Python is approximately 10-50 times slower than C++ and somewhat faster than
Matlab [table]. On recursive algorithms, it performs much better than Matlab.

A recent addition in Python is cython which turns Python code to equivalent C
code. This often does magic to a slow programs. You get speed of C and
convenience of Python. We will conduct a workshop on Cython for those who are
dealing with programs which runs for days to give results.

The attraction of Python is not speed but the ease of programming. Its syntax is
cleaner and takes less effort to master. It syntax is cleaner than its closest
rival perl. One has to type less to do same amount of work. And what matter
most, if a language got it, Python got it. There are millions of programs
written in python waiting to be used. Python can be used for almost any purpose
easily.

Python makes you get started quickly and there are so many people willing to
help you. All it takes few hours of work to get started with basics of language.
One can then start working at the problem at hand and help others getting
started with the language.



