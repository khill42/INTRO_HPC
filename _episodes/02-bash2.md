---
title: "Basic UNIX Commands"
teaching: 0
exercises: 0
questions:
- "What is the syntax of UNIX command?"
- "How do I navigate the file system?"
- "How do I transfer files to HPC?"
- "How do I interact with files on the HPC?"
objectives:
- "Be able to construct basic UNIX commands."
- "Be able to move files to and from the remote system."
- "Be able to traverse the HPC file system."
- "Be able to interact with your files."
keypoints:
- "`scp` (The Secure Copy Program) is a standard way to securely transfer data to remote HPC systems."
- "File ownership is an important component of a shared computing space and can be controlled with `chgrp` and `chown`."
- "Scripts are *mostly* just lists of commands from the command line in the order they are to be performed."
---
> ## Key Shortcuts
> Home Directory -- the tilda symbol "~" represents your home directory, /homeN/XX/jcXXYYYY/  
> Current Directory -- a single full-stop "." represents your current working directory  
> Parent Directory -- a double full-stop ".." represents the parent of your current working directory  
> Up / Down Arrows -- can be used to scroll through previous commands, so you don't have to retype them
> SetoVI  
> Tab Completion -- the UNIX shell will attempt to autocomplete file names and program names when you hit the TAB key  
> !! Previous   
> / -- on it's own a single "/" means the root directory, otherwise it indicates a new directory.  
> sourcing   
> env  
> sudo  
{: .error}
{: .callout}

## Basic UNIX Syntax

Type the command `whoami`,
then press the ENTER key to send the command to the shell.
The command's output is the ID of the current user,
i.e.,
it shows us who the shell thinks we are:

~~~
$ whoami
~~~
{: .bash}

~~~
jcXXYYYY
~~~
{: .output}

More specifically, when we type `whoami` the shell:

1.  finds a program called `whoami`,
2.  runs that program,
3.  displays that program's output, then
4.  displays a new prompt to tell us that it's ready for more commands.


> ## Username Variation
>
> In this lesson, we have used the username `nelle` (associated
> with our hypothetical scientist Nelle) in example input and output throughout.  
> However, when
> you type this lesson's commands on your computer,
> you should see and use something different,
> namely, the username associated with the user account on your computer.  This
> username will be the output from `whoami`.  In
> what follows, `nelle` should always be replaced by that username.  
{: .callout}

> ## Unknown commands
> Remember, the Shell is a program that runs other programs rather than doing
> calculations itself. So the commands you type must be the names of existing
> programs.
> If you type the name of a program that does not exist and hit enter, you
> will see an error message similar to this:
> 
> ~~~
> $ mycommand
> ~~~
> {: .bash}
> 
> ~~~
> -bash: mycommand: command not found
> ~~~
> {: .error}
> 
> The Shell (Bash) tells you that it cannot find the program `mycommand`
> because the program you are trying to run does not exist on your computer.
> We will touch on quite a few commands in the course of this tutorial, but there
> are actually many more than we can cover here.
{: .callout}

Next,
let's find out where we are by running a command called `pwd`
(which stands for "print working directory").
At any moment,
our **current working directory**
is our current default directory,
i.e.,
the directory that the computer assumes we want to run commands in
unless we explicitly specify something else.
Here,
the computer's response is `/Users/nelle`,
which is Nelle's **home directory**:

~~~
$ pwd
~~~
{: .bash}

~~~
/Users/nelle
~~~
{: .output}

> ## Home Directory Variation
>
> The home directory path will look different on different operating systems.
> On Linux it may look like `/home/nelle`,
> and on Windows it will be similar to `C:\Documents and Settings\nelle` or
> `C:\Users\nelle`.  
> (Note that it may look slightly different for different versions of Windows.)
> In future examples, we've used Mac output as the default - Linux and Windows
> output may differ slightly, but should be generally similar.  
{: .callout}

To understand what a "home directory" is,
let's have a look at how the file system as a whole is organized.  For the
sake of this example, we'll be
illustrating the filesystem on our scientist Nelle's computer.  After this
illustration, you'll be learning commands to explore your own filesystem,
which will be constructed in a similar way, but not be exactly identical.  

On Nelle's computer, the filesystem looks like this:

![The File System](../fig/filesystem.svg)

At the top is the **root directory**
that holds everything else.
We refer to it using a slash character `/` on its own;
this is the leading slash in `/Users/nelle`.

Inside that directory are several other directories:
`bin` (which is where some built-in programs are stored),
`data` (for miscellaneous data files),
`Users` (where users' personal directories are located),
`tmp` (for temporary files that don't need to be stored long-term),
and so on.  

We know that our current working directory `/Users/nelle` is stored inside `/Users`
because `/Users` is the first part of its name.
Similarly,
we know that `/Users` is stored inside the root directory `/`
because its name begins with `/`.

> ## Slashes
>
> Notice that there are two meanings for the `/` character.
> When it appears at the front of a file or directory name,
> it refers to the root directory. When it appears *inside* a name,
> it's just a separator.
{: .callout}

Underneath `/Users`,
we find one directory for each user with an account on Nelle's machine,
her colleagues the Mummy and Wolfman.  

![Home Directories](../fig/home-directories.svg)

The Mummy's files are stored in `/Users/imhotep`,
Wolfman's in `/Users/larry`,
and Nelle's in `/Users/nelle`.  Because Nelle is the user in our
examples here, this is why we get `/Users/nelle` as our home directory.  
Typically, when you open a new command prompt you will be in
your home directory to start.  

As part of going further with exploring the Bash Shell we'll move to connecting to a remote system.  This will allow us to see some new commands, ensure that the experience of each user from here on with the file system they are interacting with is (mostly) uniform, and let us begin to get familiar with the HPC system that we will be working with.


## Telling the Difference between the Local Terminal and the Remote Terminal

You may have noticed that the prompt changed when you logged into the remote system using the terminal (if you logged in using PuTTY this will not apply because it does not offer a local terminal).  This change is important because it makes it clear on which system the commands you type will be run when you pass them into the terminal.  This change is also a small complication that we will need to navigate throughout the workshop.  Exactly what is reported before the `$` in the terminal when it is connected to the local system and the remote system will typically be different for every user.  We still need to indicate which system we are entering commands on though so we will adopt the following convention:

`[local]$` when the command is to be entered on a terminal connected to your local computer

`[remote]$` when the command is to be entered on a terminal connected to the remote system

`$` when it really doesn't matter which system the terminal is connected to.

> ## Being Certain Which System your Terminal is connected to
> If you ever need to be certain which system a terminal you are using is connected to then use the follwing command: `$ hostname`.
{: .callout}

> ## Keep Two Terminal Windows Open
> It is strongly recommended that you have two terminals open, one connected to the local system and one connected to the remote system, that you can switch back and forth between.  If you only use one terminal window then you will need to reconnect to the remote system using one of the methods above when you see a change from `[local]$` to `[remote]$` and disconnect when you see the reverse.
{: .callout}

## Navigating the Remote System

Now let's learn the command that will let us see the contents of the remote filesystem.  We can see what's in our home directory by running `ls`,
which stands for "listing":

~~~
[remote]$ ls
~~~
{: .bash}

~~~
project  projects  scratch
~~~
{: .output}

(Again, your results may be slightly different depending on the operating
system and how the remote system has been customized.)


`ls` prints the names of the files and directories in the current directory in
alphabetical order,
arranged neatly into columns.

Running `ls` on the remote system is likely significantly different from what you would see if you ran `ls` from your laptop or home computer.

 where 
{: .output}

> ## Differences between remote and local system
>
> Open a second terminal window on your local computer and run the `ls` command without logging in remotely.
>  What differences do you see?
> > you would likely see something more like this:
> > ~~~
> > Applications Documents    Library      Music        > > Public
> > Desktop      Downloads    Movies       Pictures
> > ~~~
> > In addition you should also note that the preamble before the prompt (`$`) is different.  This is very important for making sure you know what system you are issuing commands on when in the shell.
> {: .solution}
{: .challenge}

We can make its output more comprehensible by using the **flag** `-F`,
which tells `ls` to add a trailing `/` to the names of directories:

~~~
[remote]$ ls -F
~~~
{: .bash}

~~~
project/  projects/  scratch/
~~~
{: .output}

# **FROM HERE DOWN NEEDS TWEAKING TO FIT WITH A REMOTE HPC SYSTEM.  PROJECT, SCRATCH, etc. NEED DESCRIBING.** 

`ls` has lots of other options. To find out what they are, we can type:

~~~
[remote]$ ls --help
~~~
{: .bash}

~~~
Usage: ls [OPTION]... [FILE]...
List information about the FILEs (the current directory by default).
Sort entries alphabetically if none of -cftuvSUX nor --sort is specified.

Mandatory arguments to long options are mandatory for short options too.
  -a, --all                  do not ignore entries starting with .
  -A, --almost-all           do not list implied . and ..
      --author               with -l, print the author of each file
  -b, --escape               print C-style escapes for nongraphic characters
      --block-size=SIZE      scale sizes by SIZE before printing them; e.g.,
                               '--block-size=M' prints sizes in units of
                               1,048,576 bytes; see SIZE format below
  -B, --ignore-backups       do not list implied entries ending with ~
  -c                         with -lt: sort by, and show, ctime (time of last
                               modification of file status information);
                               with -l: show ctime and sort by name;
                               otherwise: sort by ctime, newest first
  -C                         list entries by columns
      --color[=WHEN]         colorize the output; WHEN can be 'always' (default
                               if omitted), 'auto', or 'never'; more info below
  -d, --directory            list directories themselves, not their contents
  -D, --dired                generate output designed for Emacs' dired mode
  -f                         do not sort, enable -aU, disable -ls --color
  -F, --classify             append indicator (one of */=>@|) to entries
      --file-type            likewise, except do not append '*'
      --format=WORD          across -x, commas -m, horizontal -x, long -l,
                               single-column -1, verbose -l, vertical -C
      --full-time            like -l --time-style=full-iso
  -g                         like -l, but do not list owner
      --group-directories-first
                             group directories before files;
                               can be augmented with a --sort option, but any
                               use of --sort=none (-U) disables grouping
  -G, --no-group             in a long listing, don't print group names
  -h, --human-readable       with -l and/or -s, print human readable sizes
                               (e.g., 1K 234M 2G)
      --si                   likewise, but use powers of 1000 not 1024
  -H, --dereference-command-line
                             follow symbolic links listed on the command line
      --dereference-command-line-symlink-to-dir
                             follow each command line symbolic link
                               that points to a directory
      --hide=PATTERN         do not list implied entries matching shell PATTERN
                               (overridden by -a or -A)
      --indicator-style=WORD  append indicator with style WORD to entry names:
                               none (default), slash (-p),
                               file-type (--file-type), classify (-F)
  -i, --inode                print the index number of each file
  -I, --ignore=PATTERN       do not list implied entries matching shell PATTERN
  -k, --kibibytes            default to 1024-byte blocks for disk usage
  -l                         use a long listing format
  -L, --dereference          when showing file information for a symbolic
                               link, show information for the file the link
                               references rather than for the link itself
  -m                         fill width with a comma separated list of entries
  -n, --numeric-uid-gid      like -l, but list numeric user and group IDs
  -N, --literal              print raw entry names (don't treat e.g. control
                               characters specially)
  -o                         like -l, but do not list group information
  -p, --indicator-style=slash
                             append / indicator to directories
  -q, --hide-control-chars   print ? instead of nongraphic characters
      --show-control-chars   show nongraphic characters as-is (the default,
                               unless program is 'ls' and output is a terminal)
  -Q, --quote-name           enclose entry names in double quotes
      --quoting-style=WORD   use quoting style WORD for entry names:
                               literal, locale, shell, shell-always,
                               shell-escape, shell-escape-always, c, escape
  -r, --reverse              reverse order while sorting
  -R, --recursive            list subdirectories recursively
  -s, --size                 print the allocated size of each file, in blocks
  -S                         sort by file size, largest first
      --sort=WORD            sort by WORD instead of name: none (-U), size (-S),
                               time (-t), version (-v), extension (-X)
      --time=WORD            with -l, show time as WORD instead of default
                               modification time: atime or access or use (-u);
                               ctime or status (-c); also use specified time
                               as sort key if --sort=time (newest first)
      --time-style=STYLE     with -l, show times using style STYLE:
                               full-iso, long-iso, iso, locale, or +FORMAT;
                               FORMAT is interpreted like in 'date'; if FORMAT
                               is FORMAT1<newline>FORMAT2, then FORMAT1 applies
                               to non-recent files and FORMAT2 to recent files;
                               if STYLE is prefixed with 'posix-', STYLE
                               takes effect only outside the POSIX locale
  -t                         sort by modification time, newest first
  -T, --tabsize=COLS         assume tab stops at each COLS instead of 8
  -u                         with -lt: sort by, and show, access time;
                               with -l: show access time and sort by name;
                               otherwise: sort by access time, newest first
  -U                         do not sort; list entries in directory order
  -v                         natural sort of (version) numbers within text
  -w, --width=COLS           set output width to COLS.  0 means no limit
  -x                         list entries by lines instead of by columns
  -X                         sort alphabetically by entry extension
  -Z, --context              print any security context of each file
  -1                         list one file per line.  Avoid '\n' with -q or -b
      --help     display this help and exit
      --version  output version information and exit

The SIZE argument is an integer and optional unit (example: 10K is 10*1024).
Units are K,M,G,T,P,E,Z,Y (powers of 1024) or KB,MB,... (powers of 1000).

Using color to distinguish file types is disabled both by default and
with --color=never.  With --color=auto, ls emits color codes only when
standard output is connected to a terminal.  The LS_COLORS environment
variable can change the settings.  Use the dircolors command to set it.

Exit status:
 0  if OK,
 1  if minor problems (e.g., cannot access subdirectory),
 2  if serious trouble (e.g., cannot access command-line argument).

GNU coreutils online help: <http://www.gnu.org/software/coreutils/>
Full documentation at: <http://www.gnu.org/software/coreutils/ls>
or available locally via: info '(coreutils) ls invocation'
~~~
{: .output}

Many bash commands, and programs that people have written that can be
run from within bash, support a `--help` flag to display more
information on how to use the commands or programs.

> ## Unsupported command-line options
> If you try to use an option that is not supported, `ls` and other programs
> will print an error message similar to this:
>
> ~~~
> [remote]$ ls -j
> ~~~
> {: .bash}
> 
> ~~~
> ls: invalid option -- 'j'
> Try 'ls --help' for more information.
> ~~~
> {: .error}
{: .callout}

For more information on how to use `ls` we can type `man ls`.
`man` is the Unix "manual" command:
it prints a description of a command and its options,
and (if you're lucky) provides a few examples of how to use it.

To navigate through the `man` pages,
you may use the up and down arrow keys to move line-by-line,
or try the "b" and spacebar keys to skip up and down by full page.
Quit the `man` pages by typing "q".

Here,
we can see that our home directory contains mostly **sub-directories**.
Any names in your output that don't have trailing slashes,
are plain old **files**.
And note that there is a space between `ls` and `-F`:
without it,
the shell thinks we're trying to run a command called `ls-F`,
which doesn't exist.

> ## Parameters vs. Arguments
>
> According to [Wikipedia](https://en.wikipedia.org/wiki/Parameter_(computer_programming)#Parameters_and_arguments),
> the terms **argument** and **parameter**
> mean slightly different things.
> In practice,
> however,
> most people use them interchangeably
> to refer to the input term(s) given to a command.
> Consider the example below:
> ```
> [remote]$ ls -lh Documents
> ```
> {: .bash}
> `ls` is the command, `-lh` are the flags (also called options),
> and `Documents` is the argument.
{: .callout}

We can also use `ls` to see the contents of a different directory.  Let's take a
look at our `Desktop` directory by running `ls -F Desktop`,
i.e.,
the command `ls` with the **arguments** `-F` and `Desktop`.
The second argument --- the one *without* a leading dash --- tells `ls` that
we want a listing of something other than our current working directory:

~~~
[remote]$ ls -F Desktop
~~~
{: .bash}

~~~
data-shell/
~~~
{: .output}

Your output should be a list of all the files and sub-directories on your
Desktop, including the `data-shell` directory you downloaded at
the start of the lesson.  Take a look at your Desktop to confirm that
your output is accurate.  

As you may now see, using a bash shell is strongly dependent on the idea that
your files are organized in an hierarchical file system.  
Organizing things hierarchically in this way helps us keep track of our work:
it's possible to put hundreds of files in our home directory,
just as it's possible to pile hundreds of printed papers on our desk,
but it's a self-defeating strategy.

Now that we know the `data-shell` directory is located on our Desktop, we
can do two things.  

First, we can look at its contents, using the same strategy as before, passing
a directory name to `ls`:

~~~
[remote]$ ls -F Desktop/data-shell
~~~
{: .bash}

~~~
creatures/          molecules/          notes.txt           solar.pdf
data/               north-pacific-gyre/ pizza.cfg           writing/
~~~
{: .output}

Second, we can actually change our location to a different directory, so
we are no longer located in
our home directory.  

The command to change locations is `cd` followed by a
directory name to change our working directory.
`cd` stands for "change directory",
which is a bit misleading:
the command doesn't change the directory,
it changes the shell's idea of what directory we are in.

Let's say we want to move to the `data` directory we saw above.  We can
use the following series of commands to get there:

~~~
[remote]$ cd Desktop
[remote]$ cd data-shell
[remote]$ cd data
~~~
{: .bash}

These commands will move us from our home directory onto our Desktop, then into
the `data-shell` directory, then into the `data` directory.  `cd` doesn't print anything,
but if we run `pwd` after it, we can see that we are now
in `/Users/nelle/Desktop/data-shell/data`.
If we run `ls` without arguments now,
it lists the contents of `/Users/nelle/Desktop/data-shell/data`,
because that's where we now are:

~~~
[remote]$ pwd
~~~
{: .bash}

~~~
/Users/nelle/Desktop/data-shell/data
~~~
{: .output}

~~~
[remote]$ ls -F
~~~
{: .bash}

~~~
amino-acids.txt   elements/     pdb/	        salmon.txt
animals.txt       morse.txt     planets.txt     sunspot.txt
~~~
{: .output}

We now know how to go down the directory tree, but
how do we go up?  We might try the following:

~~~
[remote]$ cd data-shell
~~~
{: .bash}

~~~
-bash: cd: data-shell: No such file or directory
~~~
{: .error}

But we get an error!  Why is this?  

With our methods so far,
`cd` can only see sub-directories inside your current directory.  There are
different ways to see directories above your current location; we'll start
with the simplest.  

There is a shortcut in the shell to move up one directory level
that looks like this:

~~~
[remote]$ cd ..
~~~
{: .bash}

`..` is a special directory name meaning
"the directory containing this one",
or more succinctly,
the **parent** of the current directory.
Sure enough,
if we run `pwd` after running `cd ..`, we're back in `/Users/nelle/Desktop/data-shell`:

~~~
[remote]$ pwd
~~~
{: .bash}

~~~
/Users/nelle/Desktop/data-shell
~~~
{: .output}

The special directory `..` doesn't usually show up when we run `ls`.  If we want
to display it, we can give `ls` the `-a` flag:

~~~
[remote]$ ls -F -a
~~~
{: .bash}

~~~
./                  creatures/          notes.txt
../                 data/               pizza.cfg
.bash_profile       molecules/          solar.pdf
Desktop/            north-pacific-gyre/ writing/
~~~
{: .output}

`-a` stands for "show all";
it forces `ls` to show us file and directory names that begin with `.`,
such as `..` (which, if we're in `/Users/nelle`, refers to the `/Users` directory)
As you can see,
it also displays another special directory that's just called `.`,
which means "the current working directory".
It may seem redundant to have a name for it,
but we'll see some uses for it soon.

Note that in most command line tools, multiple arguments can be combined 
with a single `-` and no spaces between the arguments: `ls -F -a` is 
equivalent to `ls -Fa`.

> ## Other Hidden Files
>
> In addition to the hidden directories `..` and `.`, you may also see a file
> called `.bash_profile`. This file usually contains shell configuration
> settings. You may also see other files and directories beginning
> with `.`. These are usually files and directories that are used to configure
> different programs on your computer. The prefix `.` is used to prevent these
> configuration files from cluttering the terminal when a standard `ls` command
> is used.
{: .callout}

> ## Orthogonality
>
> The special names `.` and `..` don't belong to `cd`;
> they are interpreted the same way by every program.
> For example,
> if we are in `/Users/nelle/data`,
> the command `ls ..` will give us a listing of `/Users/nelle`.
> When the meanings of the parts are the same no matter how they're combined,
> programmers say they are **orthogonal**:
> Orthogonal systems tend to be easier for people to learn
> because there are fewer special cases and exceptions to keep track of.
{: .callout}

These then, are the basic commands for navigating the filesystem on your computer:
`pwd`, `ls` and `cd`.  Let's explore some variations on those commands.  What happens
if you type `cd` on its own, without giving
a directory?  

~~~
[remote]$ cd
~~~
{: .bash}

How can you check what happened?  `pwd` gives us the answer!  

~~~
[remote]$ pwd
~~~
{: .bash}

~~~
/Users/nelle
~~~
{: .output}

It turns out that `cd` without an argument will return you to your home directory,
which is great if you've gotten lost in your own filesystem.  

Let's try returning to the `data` directory from before.  Last time, we used
three commands, but we can actually string together the list of directories
to move to `data` in one step:

~~~
[remote]$ cd Desktop/data-shell/data
~~~
{: .bash}

Check that we've moved to the right place by running `pwd` and `ls -F`  

If we want to move up one level from the data directory, we could use `cd ..`.  But
there is another way to move to any directory, regardless of your
current location.  

So far, when specifying directory names, or even a directory path (as above),
we have been using **relative paths**.  When you use a relative path with a command
like `ls` or `cd`, it tries to find that location  from where we are,
rather than from the root of the file system.  

However, it is possible to specify the **absolute path** to a directory by
including its entire path from the root directory, which is indicated by a
leading slash.  The leading `/` tells the computer to follow the path from
the root of the file system, so it always refers to exactly one directory,
no matter where we are when we run the command.

This allows us to move to our `data-shell` directory from anywhere on
the filesystem (including from inside `data`).  To find the absolute path
we're looking for, we can use `pwd` and then extract the piece we need
to move to `data-shell`.  

~~~
[remote]$ pwd
~~~
{: .bash}

~~~
/Users/nelle/Desktop/data-shell/data
~~~
{: .output}

~~~
[remote]$ cd /Users/nelle/Desktop/data-shell
~~~
{: .bash}

Run `pwd` and `ls -F` to ensure that we're in the directory we expect.  

> ## Two More Shortcuts
>
> The shell interprets the character `~` (tilde) at the start of a path to
> mean "the current user's home directory". For example, if Nelle's home
> directory is `/Users/nelle`, then `~/data` is equivalent to
> `/Users/nelle/data`. This only works if it is the first character in the
> path: `here/there/~/elsewhere` is *not* `here/there/Users/nelle/elsewhere`.
>
> Another shortcut is the `-` (dash) character.  `cd` will translate `-` into
> *the previous directory I was in*, which is faster than having to remember,
> then type, the full path.  This is a *very* efficient way of moving back
> and forth between directories. The difference between `cd ..` and `cd -` is
> that the former brings you *up*, while the latter brings you *back*. You can
> think of it as the *Last Channel* button on a TV remote.
{: .callout}

### Nelle's Pipeline: Organizing Files

Knowing just this much about files and directories,
Nelle is ready to organize the files that the protein assay machine will create.
First,
she creates a directory called `north-pacific-gyre`
(to remind herself where the data came from).
Inside that,
she creates a directory called `2012-07-03`,
which is the date she started processing the samples.
She used to use names like `conference-paper` and `revised-results`,
but she found them hard to understand after a couple of years.
(The final straw was when she found herself creating
a directory called `revised-revised-results-3`.)

> ## Sorting Output
>
> Nelle names her directories "year-month-day",
> with leading zeroes for months and days,
> because the shell displays file and directory names in alphabetical order.
> If she used month names,
> December would come before July;
> if she didn't use leading zeroes,
> November ('11') would come before July ('7'). Similarly, putting the year first
> means that June 2012 will come before June 2013.
{: .callout}

Each of her physical samples is labelled according to her lab's convention
with a unique ten-character ID,
such as "NENE01729A".
This is what she used in her collection log
to record the location, time, depth, and other characteristics of the sample,
so she decides to use it as part of each data file's name.
Since the assay machine's output is plain text,
she will call her files `NENE01729A.txt`, `NENE01812A.txt`, and so on.
All 1520 files will go into the same directory.

Now in her current directory `data-shell`,
Nelle can see what files she has using the command:

~~~
[remote]$ ls north-pacific-gyre/2012-07-03/
~~~
{: .bash}

This is a lot to type,
but she can let the shell do most of the work through what is called **tab completion**.
If she types:

~~~
[remote]$ ls nor
~~~
{: .bash}

and then presses tab (the tab key on her keyboard),
the shell automatically completes the directory name for her:

~~~
[remote]$ ls north-pacific-gyre/
~~~
{: .bash}

If she presses tab again,
Bash will add `2012-07-03/` to the command,
since it's the only possible completion.
Pressing tab again does nothing,
since there are 19 possibilities;
pressing tab twice brings up a list of all the files,
and so on.
This is called **tab completion**,
and we will see it in many other tools as we go on.

> ## Absolute vs Relative Paths
>
> Starting from `/Users/amanda/data/`,
> which of the following commands could Amanda use to navigate to her home directory,
> which is `/Users/amanda`?
>
> 1. `cd .`
> 2. `cd /`
> 3. `cd /home/amanda`
> 4. `cd ../..`
> 5. `cd ~`
> 6. `cd home`
> 7. `cd ~/data/..`
> 8. `cd`
> 9. `cd ..`
>
> > ## Solution
> > 1. No: `.` stands for the current directory.
> > 2. No: `/` stands for the root directory.
> > 3. No: Amanda's home directory is `/Users/amanda`.
> > 4. No: this goes up two levels, i.e. ends in `/Users`.
> > 5. Yes: `~` stands for the user's home directory, in this case `/Users/amanda`.
> > 6. No: this would navigate into a directory `home` in the current directory if it exists.
> > 7. Yes: unnecessarily complicated, but correct.
> > 8. Yes: shortcut to go back to the user's home directory.
> > 9. Yes: goes up one level.
> {: .solution}
{: .challenge}

> ## Relative Path Resolution
>
> Using the filesystem diagram below, if `pwd` displays `/Users/thing`,
> what will `ls -F ../backup` display?
>
> 1.  `../backup: No such file or directory`
> 2.  `2012-12-01 2013-01-08 2013-01-27`
> 3.  `2012-12-01/ 2013-01-08/ 2013-01-27/`
> 4.  `original/ pnas_final/ pnas_sub/`
>
> ![File System for Challenge Questions](../fig/filesystem-challenge.svg)
>
> > ## Solution
> > 1. No: there *is* a directory `backup` in `/Users`.
> > 2. No: this is the content of `Users/thing/backup`,
> >    but with `..` we asked for one level further up.
> > 3. No: see previous explanation.
> > 4. Yes: `../backup/` refers to `/Users/backup/`.
> {: .solution}
{: .challenge}

> ## `ls` Reading Comprehension
>
> Assuming a directory structure as in the above Figure
> (File System for Challenge Questions), if `pwd` displays `/Users/backup`,
> and `-r` tells `ls` to display things in reverse order,
> what command will display:
>
> ~~~
> pnas_sub/ pnas_final/ original/
> ~~~
> {: .output}
>
> 1.  `ls pwd`
> 2.  `ls -r -F`
> 3.  `ls -r -F /Users/backup`
> 4.  Either #2 or #3 above, but not #1.
>
> > ## Solution
> >  1. No: `pwd` is not the name of a directory.
> >  2. Yes: `ls` without directory argument lists files and directories
> >     in the current directory.
> >  3. Yes: uses the absolute path explicitly.
> >  4. Correct: see explanations above.
> {: .solution}
{: .challenge}

> ## Exploring More `ls` Arguments
>
> What does the command `ls` do when used with the `-l` and `-h` arguments?
>
> Some of its output is about properties that we do not cover in this lesson (such
> as file permissions and ownership), but the rest should be useful
> nevertheless.
>
> > ## Solution
> > The `-l` arguments makes `ls` use a **l**ong listing format, showing not only
> > the file/directory names but also additional information such as the file size
> > and the time of its last modification. The `-h` argument makes the file size
> > "**h**uman readable", i.e. display something like `5.3K` instead of `5369`.
> {: .solution}
{: .challenge}

> ## Listing Recursively and By Time
>
> The command `ls -R` lists the contents of directories recursively, i.e., lists
> their sub-directories, sub-sub-directories, and so on in alphabetical order
> at each level. The command `ls -t` lists things by time of last change, with
> most recently changed files or directories first.
> In what order does `ls -R -t` display things? Hint: `ls -l` uses a long listing
> format to view timestamps.
>
> > ## Solution
> > The directories are listed alphabetical at each level, the files/directories
> > in each directory are sorted by time of last change.
> {: .solution}
{: .challenge}

## Making files to practice with
Before we start moving files around we should have some files to move around that don't really matter beyond their value in being moved around.  If they get lost or damaged or anything else it won't really matter because they are mostly harmless (we say "mostly" because it is possible that they might add clutter or overwrite another file with the same name but we will take steps to avoid these risks).  We will create such files using the `touch` command after using `cd` to make sure that we are in the home directory.

~~~
[remote]$ cd
[remote]$ touch fileToMove
~~~
{: .bash}

~~~
[remote]$ ls
~~~
{: .bash}

~~~
...
fileToMove
...
~~~
{: .output}

We now have a file called "fileToMove" that we can move around.  If you run an `ls -l` you will note that its size is zero.  This is because it has no content.  This makes it a fairly safe file to move around because it can't really do anything.  It is also an easy file to move around because we won't need to worry about transfer times.

> ## Overloading Commands
> Running `$ man touch` will reveal that `touch` is the command meant to be used for changing the timestamps associated with files.  We're not using it to do this though and are instead using it to create a new file.  This is because when touch is run if it is passed the name for a file that doesn't exist it will create that file.  This is a useful feature but not what the command is specifically intended for and so we sometimes refer to this as "overloading" a command.  Using the concatentate file command `cat` to print the contents of a single file is another example of this.
{: .callout}

## Creating Directories

Now that we have a file to move around we need a place to move the file to.  We can create a new directory using the `mkdir` command:

~~~
[remote]$ mkdir practiceDir
[remote]$ ls
~~~
{: .bash}

~~~
...
practiceDir
...
~~~
{: .output}

We can check the content of the new directory as well:

~~~
[remote]$ ls practiceDir
~~~
{: .bash}

~~~

~~~
{: .output}

We don't have any output returned, which is to be expected because we only just created the directory and nothing has been added to it yet.

## Moving Files Around

We can move our new file into the new directory with the move command, `mv`.  The syntax of `mv` is `$ mv file_being_moved location_moving_to`.   Moving our new file "fileToMove" to our new directory "practiceDir" can be done as follows:

~~~
[remote]$ mv fileToMove practiceDir
~~~
{: .bash}

"Silence is Golden" so as long as the move was successful we will receive no output.  We can check that the file was moved successfully using `ls`:

~~~
[remote]$ ls practiceDir
~~~
{: .bash}

~~~
fileToMove
~~~
{: .output}

We can move `fileToMove` back from `practiceDir` as well:

~~~
[remote]$ mv practiceDir/fileToMove fileToMove
~~~
{: .bash}

Again, if the transfer is successful then we will receive no output but we can use `ls` to check.

~~~
[remote]$ ls
~~~
{: .bash}

~~~
fileToMove
~~~
{: .output}

~~~
[remote]$ ls practiceDir
~~~
{: .bash}

~~~

~~~
{: .output}
 
## Copying Files
 
We can also copy files, leaving the original file while a second version is created either elsewhere or in the same location.  The copy command is `cp` and its syntax is the same as for `mv`: `$ cp file_being_copied location_copying_to`.  We can create a copy of "fileToMove" inside of the "practiceDir" directory as follows:

~~~
[remote]$ cp fileToMove practiceDir
~~~
{: .bash}

"Silence is Golden" so as long as the copy was successful we will receive no output.  We can check that the file was copied successfully using `ls`:

~~~
[remote]$ ls
~~~
{: .bash}

~~~
...
fileToMove
...
~~~
{: .output}

~~~
[remote]$ ls practiceDir
~~~
{: .bash}

~~~
fileToMove
~~~
{: .output}

## Removing Files and Directories

Sooner or later it will become important to be able to remove files and directories from a system via the command line.  This is usually done with the remove command, `rm`.  The syntax of `rm` is: `$rm optional_list_of_opitons list_of_files_to_be_removed`.  To remove "fileToMove" from our current directory, which should also be the home directory, we issue the following command:

~~~
[remote]$ rm fileToMove
~~~
{: .bash}

If the file exists and there is nothing standing in the way of your ability to delete it such as a lack of authority to do so then the `rm` command will not return any output and just remove the file.  We can confirm this with the `ls` command.

We can also remove entire directories with `rm`.  Try this now with `practiceDir`:

~~~
[remote]$ rm practiceDir
~~~
{: .bash}

~~~
rm: cannot remove 'practiceDir/': Is a directory
~~~
{: .output}

This will notably fail with an error declaring that `rm` cannot remove "practiceDir" because it is a directory.  This is not actually the case. `rm` is quite capable of removing directories, we just need to pass the appropriate options.  The option we want here is `-r` with turns on recursion, allowing the removal of all the content within a directory and any directories and files within that directory and any directories and files within those directories and so on.  Let's try again with recursion flag:

~~~
[remote]$ rm -r practiceDir
~~~
{: .bash}

The directory named "practiceDir" is now gone and so is the file named "fileToMove" that was inside it.

> ## No Trash Can or Recycle Bin
> If you are used to working with graphic user interfaces such as Windows, Mac OSX, or X-Windows on Linux then you are likely used to there being a special folder that holds any files that you delete for at least a short period of time before they are gone for good.  This feature does not exist in most command line environments.  When you use `rm` to remove a file it really is removed, short of a forensic audit of the filesystem.  Be very careful!
{: .callout}


## Moving files to and from the remote system from and to your local computer

It is often necessary to move data from your local computer to the remote system and vice versa.  There are many ways to do this and we will look at two here: `scp` and `sftp`.

### `scp` from your local computer to the remote system
The most basic command line tool for moving files around is secure copy or `scp`.

`scp` behaves similarily to `ssh` but with one additional input, the name of the file to be copied.  If we were in the shell on our local computer, the file we wanted to move was in our current directory, named "globus.tgz", and Nelle wanted to move it to her home directory on cedar.computecanada.ca then the command would be
	
~~~
[local]$ scp fileToMove nelle@cedar.computecanada.ca:
~~~
{: .bash}
	
It should be expected that a password will be asked for and you should be prepared to provide it.

Once the transfer is complete you should be able to use `ssh` to login to the remote system and see your file in your home directory.

~~~
[remote]$ ls
~~~
{: .bash}

~~~
...
fileToMove
...
~~~
{: .output}

WHAT IF YOU WANTED THE FILE SOMEWHERE OTHER THAN THE HOME DIRECTORY?

> ## Remember the `:`
> Note the colon (`:`) at the end of the command.  This is a very important modifier to include.  If it isn't included then running `scp` will result in a copy of the file that was to be moved being created in the current directory with the name of the remote system.  In the case of the globus.tgz example above it would look like the following:
> 
> ~~~
> [local]$ scp globus.tgz nelle@cedar.computecanada.ca: 
> [local]$ ls
> ~~~
> {: .bash}
> 
> ~~~
> globus.tgz
> nelle@cedar.computecanada.ca
> ~~~
> {: .output}
> 
> If this does happen then the extra file can be removed with `rm`.
> 
> ~~~
> [local]$ rm nelle@cedar.computecanada.ca
> ~~~
> {: .bash}
{: .callout}

NEED AN EXERCISE.  OPEN A SECOND TERMINAL WINDOW ON LOCAL SYSTEM AND USE `scp` TO PUSH A FILE TO THE REMOTE SYSTEM

### `scp	` from the remote system to your Local computer

Moving files from the remote system to the local system will also be necessary.  This is also done from the local system so instead of pushing content we'll be pulling it.  This is achieved by changing the order of the inputs passed to the `scp` command and specifying the exact location of the file that we want from the remote system.

scp simpson@cedar.computecanada.ca:junk junk

> ## Copying entire directories
> 
> HIGHLIGHT the `-r` flag here.  Note its similarity to `-r` with `rm`.
> 
{: .callout}

## Transferring files interactively with sftp

`scp` is useful, but what if we don't know the exact location of what we want to transfer?
Or perhaps we're simply not sure which files we want to tranfer yet.
`sftp` is an interactive way of downloading and uploading files.
To connect to a cluster, using `sftp` the syntax is: `sftp yourUsername@remote.computer.address`.
This will start what appears to be a Bash shell but that is really the sftp program.  We know that we are inside the sftp program because the prompt changes to `sftp>`.  While `sftp` looks like a Bash shell we only have access to a limited number of commands.

We can see which commands are available with `help`:
```
sftp> help
```
{: .bash}
```
Available commands:
bye                                Quit sftp
cd path                            Change remote directory to 'path'
chgrp grp path                     Change group of file 'path' to 'grp'
chmod mode path                    Change permissions of file 'path' to 'mode'
chown own path                     Change owner of file 'path' to 'own'
df [-hi] [path]                    Display statistics for current directory or
                                   filesystem containing 'path'
exit                               Quit sftp
get [-afPpRr] remote [local]       Download file
reget [-fPpRr] remote [local]      Resume download file
reput [-fPpRr] [local] remote      Resume upload file
help                               Display this help text
lcd path                           Change local directory to 'path'
lls [ls-options [path]]            Display local directory listing
lmkdir path                        Create local directory
ln [-s] oldpath newpath            Link remote file (-s for symlink)
lpwd                               Print local working directory
ls [-1afhlnrSt] [path]             Display remote directory listing

# omitted further output for brevity
```
{: .output}

Notice the presence of multiple commands that make mention of local and remote.
We are actually connected to two computers at once (with two working directories!).

To show our remote working directory:
```
sftp> pwd
```
{: .bash}
```
Remote working directory: /global/home/yourUsername
```
{: .output}

To show our local working directory, we add an `l` in front of the command:

```
sftp> lpwd
```
{: .bash}
```
Local working directory: /home/jeff/Documents/teaching/hpc-intro
```
{: .output}

The same pattern follows for all other commands:

* `ls` shows the contents of our remote directory, while `lls` shows our local directory contents.
* `cd` changes the remote directory, `lcd` changes the local one.

To upload a file, we type `put some-file.txt` (tab-completion works here).

```
sftp> put config.toml
```
{: .bash}
```
Uploading config.toml to /global/home/yourUsername/config.toml
config.toml                                   100%  713     2.4KB/s   00:00 
```
{: .output}

To download a file we type `get some-file.txt`:

```
sftp> get config.toml
```
{: .bash}
```
Fetching /global/home/yourUsername/config.toml to config.toml
/global/home/yourUsername/config.toml                               100%  713     9.3KB/s   00:00 
```
{: .output}

And we can recursively put/get files by just adding `-r`.
Note that the directory needs to be present beforehand.

```
sftp> mkdir content
sftp> put -r content/
```
{: .bash}
```
Uploading content/ to /global/home/yourUsername/content
Entering content/
content/scheduler.md              100%   11KB  21.4KB/s   00:00    
content/index.md                  100% 1051     7.2KB/s   00:00    
content/transferring-files.md     100% 6117    36.6KB/s   00:00    
content/.transferring-files.md.sw 100%   24KB  28.4KB/s   00:00    
content/cluster.md                100% 5542    35.0KB/s   00:00    
content/modules.md                100%   17KB 158.0KB/s   00:00    
content/resources.md              100% 1115    29.9KB/s   00:00    
```
{: .output}

To quit, we type `exit` or `bye`.

## Grabbing files from the internet

To download files from the internet, 
the absolute best tool is `wget`.
The syntax is relatively straightforwards: `wget https://some/link/to/a/file.tar.gz`

> ## Downloading the Drosophila genome
> The *Drosophila melanogaster* reference genome is located at the following website:
> [http://metazoa.ensembl.org/Drosophila_melanogaster/Info/Index](http://metazoa.ensembl.org/Drosophila_melanogaster/Info/Index).
> Download it to the cluster with `wget`.
>
> Some additional details:
>
> * You want to go get the genome through the "Download DNA Sequence" link
> * We are interested in the `Drosophila_melanogaster.BDGP6.dna.toplevel.fa.gz` file.
{: .challenge}

> ## Working with compressed files
> 
> The file we just downloaded is gzipped (has the `.gz` 
> extension).
>You can uncompress it with `gunzip filename.gz`.
>
>File decompression reference:
>
>* **.tar.gz** - `tar -xzvf archive-name.tar.gz`
>* **.tar.bz2** - `tar -xjvf archive-name.tar.bz2`
>* **.zip** - `unzip archive-name.zip`
>* **.rar** - `unrar archive-name.rar`
>* **.7z** - `7z x archive-name.7z`
>
>However, sometimes we will want to compress files 
>ourselves to make file transfers easier.
>The larger the file, the longer it will take to 
>transfer. 
>Moreover, we can compress a whole bunch of little 
>files into one big file to make it easier
>on us (no one likes transferring 70000) little files!
>
>The two compression commands we'll probably want to 
>remember are the following:
>
>* Compress a single file with Gzip - `gzip filename`
>* Compress a lot of files/folders with Gzip - `tar -czvf archive-name.tar.gz folder1 file2 folder3 etc`
> 
{: .callout}

## Scripting

We need a short script and a brief explanation on `nano`.  Or we could use Jeff's simple script (below).

> ## Creating our test job
> 
> 
>
>```
>#!/bin/bash
>
> echo 'This script is running on:'
> hostname
> sleep 120
>```
{: .challenge}

nano

<!--

Not sure if we want this content.  Likely should be driven by the use case.

## Controlling File Ownership

When working on an HPC system you will likely find yourself working with files that you need to either take ownership of or assign ownership to.


List with `ls -l`

Change ownership with `chown`

## Groups

## Changing Group Association

chgrp

## Making Scripts Executable / Changing Permissions

chmod

p. 195
-->