---
title: Using python for scripting
author: Dilawar Singh
published: true
status: publish
layout: post
type: post
tags: [Python]
categories: [Programming, Python, Scripting]
comments: true
---

I am one of those who prefer bash over anything else for system scripting. Bash
is minimal and I love this about it. In bash, one deals with only text although
some primitive data-structures such as arrays are also available. Bash is one of
those things which proves that so much can be accomplished by a minimal tool.
But scripting is more than just managing and administering systems.

Python scripts are easy to maintain and write. They are easy because Python
comes with extensive standard library. It has great great support for reading
and parsing text files, easy to use data-structures and iterators. Moreover, a
huge user community has created a great deal of work which can be easily
plugged. I am pretty sure that Perl (given its age and usage) will also be a
great candidate.

This much is to motivate. Let's see some code so we can do at least one or two
things after reading it. But first thing first.

## Configure editor

- Replace tab character in your editor with 4 spaces.
- If you can break a line at column-width 80, do it. Don't ask why! Never make a
  line bigger than 100 characters.
- If you are not into vim or emacs, checkout kate, gedit, notepad++ etc.

Check out the [most preferred
coding-style](http://legacy.python.org/dev/peps/pep-0008/) in your language of
choice.  Readability of your code matters, whether it is your team-mates or poor
TAs checking your code.  Scripts are no exception even if you are the only one
using it. Good coding style also compensates for the lack of comments.

## Writing a standalone script

One can write a single script containing everything or break it into many
scripts. The latter is always recommended if your script is larger than few
hundreds of lines. But if you want to write a portable script which you can pass
around, writing a single scripts is not a such a bad idea. But your function
should not be big. If it is more than 50 lines of code, break it into two
functions.

Notice the first line (shebangs/hashbangs) in these scripts. Prefer `#!/usr/bin/env` 
over something like `#!/usr/bin/python`.

~~~
#!/usr/bin/env python
'''
This script can turn water into milk.
'''
def turnWaterIntoMilk():
    print("I dont know how to do it, yet!")
    return None

turnWaterIntoMilk()

~~~

Do not forget to add [`if __name__ == "__main__"`](http://stackoverflow.com/questions/419163/what-does-if-name-main-do)
to your script file.

This is how a minimal script should look like.

~~~
#!/usr/bin/env python
'''
This script can turn water into milk.
'''
def turnWaterIntoMilk():
    print("I dont know how to do it, yet!")
    return None

if __name__ == "__main__":
    turnWaterIntoMilk()

~~~

## Prefer `try/catch` 

[Beg forgiveness over asking permission.](http://programmers.stackexchange.com/questions/175655/python-forgiveness-vs-permission-and-duck-typing)
Compare this programming style with [Look Before You Leap](http://docs.python.org/2/glossary.html).

## Working with text files

To read (write) a file, prefer `with` statement.

~~~
with open("filename.txt", "r") as f:
    txt = f.read()
doSomething(txt)
~~~

Doing this is not also not a bad idea. 

~~~
txt = open("filename.txt", "r").read()
doSomething(txt)
~~~

The keyword `with` takes care of closing the file once everything is read or
written to the file. If you want to read line by line in a file, this is handy.


~~~
with open("filename.txt", "r") as f:
    for line in f:
        doSomethingWithLine(line)
~~~
Or, with a little bit of functional style makes things smaller. I just love list
comprehension.

~~~
with open("filename.txt", "r") as f:
    [ doSomethingWithLine(line) for line in f ]

~~~

## Manipulating string

Once a text-file is read, one process it contents. Some of the more basic
operations on strings are listed below.

#### Break a string at a substring

~~~~
line = "Happiness can't buy me money"
tokens = line.split()
print tokens
~~~~
This will print `['Happiness', "can't", 'buy', 'me', 'money']`. If function
`split` is given a substring, the string will be broken at given substring.

~~~
token = line.split("ne")
~~~

This gives `['Happi', "ss can't buy me mo", 'y']`. Notice that substring is
removed. If substring is not found, it does the obvious. Try it!

More complex string manipulation can be done using regular expressions. They are
powerful tools and it is very easy to write very inefficient regular
expressions. It would be a good project to write a function which minimizes a
giver regular expressions (state minimization in state-machine).

#### Replacing substring

Say you want to replace all 'ness' with 'less' in a string. 

~~~
newLine = line.replace('ness', 'less')
~~~
`newLine` is now `"Happiless can't buy me money"`. One can even chains these
operations.

~~~ 
newLine = line.replace('p', 't').replace('e', 'o') 
~~~ 

`newLine` is `Hattinoss can't buy mo monoy`. Actually
[`str.translate`](http://docs.python.org/2/library/stdtypes.html#str.translate)
is a much better way to do this though harder to remember.


## Call system command using `subprocess`

Let's take an example: use `pandoc` to convert a markown to html. The shell
command is following.

~~~
pandoc -f markdown -t html file.markdown > file.html
~~~

Equivalent in Python using `subprocess`  is following. One can also some
shorter version of this command. I prefer this because I get much finer control
over the system command. 

This is a rather involved example. Mostly one runs a command and captures its
output and decide what to do next depending on the output.

~~~
with open("file.markdown", "r" as f:
    markdown = f.read()

cmd = ["pandoc", "-f", "markdown", "-t", "html"]
p = subprocess.Popen(cmd
        , stdin = subprocess.PIPE
        , stdout = subprocess.PIPE
        )
# Write mardown content into stdin
p.stdin.write(markdown)
# Recieve html from stdout
html = p.communicate()[0]
~~~

## Regular expressions

I don't know why I am touching it.

Regular expression are like language grammar (perhaps less powerful): they
accept or reject a given string. This is very useful for making sense out of
user input or commands. They can also be used for simple parsing. You can read
about it [here](http://en.wikibooks.org/wiki/Regular_Expressions/Introduction).
Understanding regex needs some reading and getting it work in any language is
not always easy. If one can debug a wrong regex, one can debug almost anything.

I want to show it in action. If it does not make sense at all, then do read
about it.

Suppose a user gives me a line suppose to be a valid voltage source line from a
spice script. The grammar (roughly) is following (case insensitive).

> v by name of device e.g. V1, va, vn1 etc. This is followed by
> name of its terminals  and then its type (ac or dc). This is followed by its
> value in decimal (ignore 1e-5 syntax for now). Immediately after the value there
> is a character: k for kilo, m or mili, M or mega etc. For example.

~~~
line = "v1 in1 out1 dc 0.1k"
~~~

Following regular expression accepts a valid string of above grammar and rejects 
bad one.

~~~
import re
pat = re.compile(r'v(?P<name>\w+)\s+(?P<in>\w+)\s+(?P<out>\w+)\s+(?P<type>ac|dc)\s+(?P<value>\d+(\.\d+)?)\w', re.I)
m = pat.match(line)
if m:
    print("This matches. Matched values are")
    print(m.groupdict())
else:
    print("Given line is not a valid spice line for volatage source")
    raise UserWarning("Bad input")
~~~

This prints

~~~
This matches. Matched values are
{u'in': u'in1', u'type': u'dc', u'name': u'1', u'value': u'0.1', u'out': u'out1'}
~~~

Well, since we are here let's make some sense out of regex. Let's remove
'?P<someting>' from the regex, it is used to name a subsection of regex so we
can access it later. The regex is now
`v(\w+)\s+(\w+)\s+(\w+)\s+(ac|dc)\s+(\d+(\.\d+)?)\w'`.  Symbol `\w` means 'a
alphanumeric character or the underscore' `+` means one or more time i.e. `\w+`
means 'a alphanumeric character or the underscore, one or more times. Therefore
`v\w+` means character `v` followed by one or more alphanumeric character or
underscore. Next we have `\s+` which is one or more whitespaces etc. (ac|dc)
means either string `ac` or `dc`. `\d` means a number. `(\.\d+)` means a literal
`.` followed by one or more number e.g. `.99`, `.001` but it is postfixed by `?`
which make it _optional_. In nutshell if it can match `1.22` as well as `1` (.22
is optional). Phew!



