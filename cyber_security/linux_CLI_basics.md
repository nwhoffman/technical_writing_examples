# Know what standard input (stdin) and output (stdout) are and how to use redirection

## Overview

When we are working in the terminal, and using commands such as `ls`, the output is actually sent to a special file called standard output  or **stdout**. This output is linked to the screen and not actually saved to a file. But, we can do something called redirection and send the standard output to a file.

We might do an `ls` in a directory with a lot of files and directories and then be able to later search through that information. To create a file and save the output from `ls` we can use the `>` right before the file name:

```bash
$ ls -l /usr/bin > ls_output.txt
$ ls -l ls_output.txt
-rw-r--r-- 1 myusername myusername 169445 Jul  1 16:56 ls_output.txt
```
 
This will make a text file containing all of the binary program files in the /usr directory. The output file will be in the directory where you issued the `ls` command. We can see that this file is 170 KB which is a lot of text. We can use the `wc` or word count command to see how many lines are in this file:

```bash
$ wc -l ls_output.txt
2596 ls_output.txt
```
Without redirecting the output, we would have had to scroll through more than 2500 lines of text.

In Linux, we can also make use of the standard input or **stdin**. The standard input originates from the keyboard and whatever you type can be directed to a file. Let use an example with the `cat` (*concatenate*) command.

```bash
$ cat > mynewfile.txt

```
Next, whatever is typed into the terminal will be directed into the specified file:

```bash
$ cat > mynewfile.txt
Here is some text for my new file.
We can use return to make a new line.


Or a few returns to make some space.

To exit from this interactive shell, use ctrl+d

```

When we view our file (using the `cat` command again but without the `>`) we can see that what was typed into the terminal was written to the file.

```bash
$ cat mynewfile.txt
Here is some text for my new file.
We can use return to make a new line.


Or a few returns to make some space.

To exit from this interactive shell, use ctrl+d

```

	
## Follow Along

We will look at a more detailed example of creating a new file using `cat`, adding input from the keyboard, and concatenating with another file.


## Challenge

Given an example file(s) the content will be added from the keyboard, concatenated, and printed out to a new file (*or something like this*)


## Additional Resources

- [Linux Commands for Beginners](https://maker.pro/linux/tutorial/basic-linux-commands-for-beginners)
# Know how to use `grep` to perform basic pattern matching

## Overview

When working with files and text, it can be very useful to know how to search for patterns in a file name or file content. There is a tool called `grep` which matches *regular expressions*  which is a sequence of characters which defines a pattern; it is sometimes called a *regex*. The name *grep* originates from how the command is used: **g**lobally search a **re**gular expression and **p**rint.

Complex search strings can be implemented with grep, but just knowing some of the basics can be very powerful. There are a few ways to use grep, but here are some of the examples of the most common uses.

### String matching
The syntax for using `grep`is `grep string filename` where *string* is the pattern you would like to match and filname can be the path to the file. Let's use an example file of user names and their passwords. In Linux, this file is located in /etc/passwd. For example, to find users named "batman" on this particular system, the syntax would be:

```bash
$ grep batman /etc/passwd
```

*Note to writer/developer: need to generate a /etc/passwd file with example user names and encrypted passwords*

We can also match a given string for all the files in a directory by using the `*` or wildcard character. If the user has a directory called `mywriting` and we want to find all of the files with "flower" in the name, the syntax would be as follows

```bash
# Go to the home directory for user: batman
$ cd /home/batman
$ ls
batmobile batcave mywriting 

# Find all files in the mywriting directory with the word flower in the name
$ grep flower mywriting
flower_colors.txt flower_poems.txt
```
	
## Follow Along

We will use `grep`with a few more examples, including how to search recursively through directories, how to match/ignore cases, and how to use wildcards `*`

## Challenge

Using an example /etc/passwd file, you will use `grep` to find:

* how many users have *<given text>* in their name
* how to find users with a given IP address


## Additional Resources

- [grep for cyber applications](https://www.cyberciti.biz/faq/howto-use-grep-command-in-linux-unix/)
- [grep examples](https://www.thegeekstuff.com/2009/03/15-practical-unix-grep-command-examples/)
# Know how to use `head`, `tail`, `more`, `less`

## Overview

When working with files, it is helpful to be able to view and display them in different ways in the terminal. There are some useful commands that make it easy to view the entire file, in addition to just some of the parts.

### Using `head` and`tail`

These commands do what their names suggest: look at the head (beginning) and tail (end) of a file. They both display the first or last 10 lines of code by default. Let's make a file using some of the tools we've learned and look at the head and tail. 

First, we'll make a file, redirect some output into the file, and then view. The man page for bash is long:

```bash
$ touch bash_manpage.txt
$ man bash > bash_manpage.txt

# Let's check the line count
$ wc -l bash_manpage.txt
5737 bash_manpage.txt

# Look at the first 10 lines
$ head bash_manpage.txt
BASH(1)                     General Commands Manual                    BASH(1)

NAME
       bash - GNU Bourne-Again SHell

SYNOPSIS
       bash [options] [command_string | file]

COPYRIGHT
       Bash is Copyright (C) 1989-2013 by the Free Software Foundation, Inc.

# And the last 10 lines
$ tail bash_manpage.txt
process  is stopped, the shell immediately executes the next command in
       the sequence.  It suffices to place the sequence  of  commands  between
       parentheses  to  force  it  into  a subshell, which may be stopped as a
       unit.

       Array variables may not (yet) be exported.

       There may be only one active coprocess at a time.

GNU Bash 4.3                    2014 February 2                        BASH(1)

```
### Using `more` and `less`

These commands are related to each in other in that `less` is `more`. Joking aside, the `more` command displays a file one page at a time and for this reason is called a *pager*. When using `more` you can only move forward through a file. The newer command `less` works the same except you can also move backwards through the file. So, `less` is `more` but with an added feature. More or less.

With our example file, we can go through one page at a time:

```
$ more bash_manpage.txt
```
And with the `less` command we have the option to go back through the "pages":

```
$ less bash_manpage.txt
```
 	
## Follow Along

Let's use some bigger files so we can see how these commands are useful. *Note: need some good example files here*

## Challenge 

It's more fun to practice on files that are interesting to you. To have a little fun with viewing your text file, let's copy some text from a public domain book. Go to your favorite source of public domain texts, find a book you like, and copy a number of lines of the text; you don't need to copy the entire book but you can. Using nano, save this copied text, and practice the above commands.


## Additional Resources

- [Managing files (head/tail)](https://www.tecmint.com/view-contents-of-file-in-linux/)
# Know how to use `sed`to do a basic search/replace

## Instructor Notes

## Required Resources

## Overview

In addition to opening a file and editing it in the terminal, it can be useful to work with a file without opening it. The text editor `sed` or stream editor is a utility that can accept input from a file or directly from stdin.

The syntax for `sed` is:

```bash
sed [options] commands [file-to-edit]
```

where the file-to-edit can also be standard input. Using an example file, we can use `sed` to print to the screen.

```bash
$ sed '' example_file.txt
```
Because we didn't give `sed` any options, the default is to print to the screen.

```bash
example file output
```

It is also easy to search and replace words, or even letters and numbers, using the `'s'` option:

```bash
$ sed 's/oldword/newword/' examplefile.txt
```
The first word is the pattern to search for and the secod word is what to replace it with. There are many ways to construct a more complicated search and replace, but even a simple one like above is useful.

`sed` can be combined with other commands like `cat` in the example below.

```bash
cat example_file.txt | sed -e 's/foo/bar/g'
```
	
## Follow Along

Let's use `sed` to find and replace some text in a file.


## Challenge


## Additional Resources

- [The basics of using Sed](https://www.digitalocean.com/community/tutorials/the-basics-of-using-the-sed-stream-editor-to-manipulate-text-in-linux)
# Know how to use pipes with various commands


## Overview

When working with the command line, it is also useful to be able to use the output from a command as the input for another command. Pipelines or pipes for short are a way to string a number of commands together, saving you from having ot do intermediate steps like saving a file or viewing the output before you do the next command.

The commands to be executed in the pipeline are separated by the vertical bar character `|` which is called the pipe. It's usually located with the `\` key. It's easy to remember because it looks like a small pipe. There is no limit to how many commands you can string together, but stick to just a few or you won't remember what your end result was supposed to be.

The sytax is: `$ command_1 | command_2 | command_3 | .... | command_N`

It is pretty common to pipe the output from `ls` into a pager command like `less`; it's much easier to page through the output from `ls` especially if there are a large number of files and directories. Let's try out the `ls |` with the `/etc` directory since it has a lot of files:

```bash
$ ls -al /etc | less
```

![Pipe Output](../../images/PipeOutput1.png)

It's alo pretty common to use `|` with `grep`. Let's try out an example looking for all files in the `/etc` directory that  contain the string *conf* for configuration files. We will also pipe the output from `grep` into `less`:

```bash
$  ls -al /etc/ | grep conf | less
```

![Pipe Output grep](../../images/PipeOutput2.png)	

Now we can see that all of the files have *conf* in the name.	
	
## Follow Along

We'll go over a few more specific examples of how to use pipes with `locate`, `find`, and `sed`

## Challenge

When managing a network, a system administrator or security analyst will want to see when certain users last logged in. Using the example file, the `last` command and different uses of pipe, `less`, and `grep`, find the name of the user who logged in most recently.

*Note: need to generate a file of user logs, hopefully for a network and not just a personal computer*


## Additional Resources

- [Piping and redirection](https://ryanstutorials.net/linuxtutorial/piping.php)
