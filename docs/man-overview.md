# Overview

This chapter is for people who have never used **`cvs`**, and perhaps have never used version control software before.

If you are already familiar with **`cvs`** and are just trying to learn a particular feature or remember a certain command, you can probably skip everything here.

What is **`cvs`**?: What you can do with **`cvs`**
What is **`cvs`** not?: Problems **`cvs`** doesn't try to solve
A sample session: A tour of basic **`cvs`** usage

## What is cvs?

**`cvs`** is a version control system. Using it, you can record the history of your source files.
For example, bugs sometimes creep in when software is modified, and you might not detect the bug until a long time after you make the modification. With **`cvs`**, you can easily retrieve old versions to see exactly which change caused the bug. This can sometimes be a big help.

You could of course save every version of every file you have ever created. This would however waste an enormous amount of disk space. **`cvs`** stores all the versions of a file in a single file in a clever way that only stores the differences between versions.

**`cvs`** also helps you if you are part of a group of people working on the same project. It is all too easy to overwrite each others' changes unless you are extremely careful. Some editors, like gnu Emacs, try to make sure that two people never modify the same file at the same time. Unfortunately, if someone is using another editor, that safeguard will not work. **`cvs`** solves this problem by insulating the different developers from each other. Every developer works in his own directory, and **`cvs`** merges the work when each developer is done.

**`cvs`** started out as a bunch of shell scripts written by Dick Grune, posted to the newsgroup comp.sources.unix in the volume 6 release of July, 1986. While no actual code from these shell scripts is present in the current version of **`cvs`** much of the **`cvs`** conflict resolution algorithms come from them.

In April, 1989, Brian Berliner designed and coded **`cvs`**. Jeff Polk later helped Brian with the design of the **`cvs`** module and vendor branch support.

You can get **`cvs`** in a variety of ways, including free download from the Internet. For more information on downloading **`cvs`** and other **`cvs`** topics, see:

     http://cvs.nongnu.org/

There is a mailing list, known as info-cvs@nongnu.org, devoted to **`cvs`**. To subscribe or unsubscribe write to info-cvs-request@nongnu.org. If you prefer a Usenet group, there is a one-way mirror (posts to the email list are usually sent to the news group, but not visa versa) of info-cvs@nongnu.org at news:gnu.cvs.help. The right Usenet group for posts is news:comp.software.config-mgmt which is for **`cvs`** discussions (along with other configuration management systems). In the future, it might be possible to create a comp.software.config-mgmt.cvs, but probably only if there is sufficient **`cvs`** traffic on news:comp.software.config-mgmt.
You can also subscribe to the bug-cvs@nongnu.org mailing list, described in more detail in BUGS. To subscribe send mail to bug-cvs-request@nongnu.org. There is a two-way Usenet mirror (posts to the Usenet group are usually sent to the email list and visa versa) of bug-cvs@nongnu.org named news:gnu.cvs.bug.

## What is cvs not?

**`cvs`** can do a lot of things for you, but it does not try to be everything for everyone.

**`cvs`** is not a build system.
Though the structure of your repository and modules file interact with your build system (e.g. Makefiles), they are essentially independent.
**`cvs`** does not dictate how you build anything. It merely stores files for retrieval in a tree structure you devise.

**`cvs`** does not dictate how to use disk space in the checked out working directories. If you write your Makefiles or scripts in every directory so they have to know the relative positions of everything else, you wind up requiring the entire repository to be checked out.

If you modularize your work, and construct a build system that will share files (via links, mounts, VPATH in Makefiles, etc.), you can arrange your disk usage however you like.

But you have to remember that any such system is a lot of work to construct and maintain. **`cvs`** does not address the issues involved.

Of course, you should place the tools created to support such a build system (scripts, Makefiles, etc) under **`cvs`**.

Figuring out what files need to be rebuilt when something changes is, again, something to be handled outside the scope of **`cvs`**. One traditional approach is to use make for building, and use some automated tool for generating the dependencies which make uses.

See Builds, for more information on doing builds in conjunction with **`cvs`**.

**`cvs`** is not a substitute for management.
Your managers and project leaders are expected to talk to you frequently enough to make certain you are aware of schedules, merge points, branch names and release dates. If they don't, **`cvs`** can't help.
**`cvs`** is an instrument for making sources dance to your tune. But you are the piper and the composer. No instrument plays itself or writes its own music.

**`cvs`** is not a substitute for developer communication.
When faced with conflicts within a single file, most developers manage to resolve them without too much effort. But a more general definition of “conflict” includes problems too difficult to solve without communication between developers.
**`cvs`** cannot determine when simultaneous changes within a single file, or across a whole collection of files, will logically conflict with one another. Its concept of a conflict is purely textual, arising when two changes to the same base file are near enough to spook the merge (i.e. diff3) command.

**`cvs`** does not claim to help at all in figuring out non-textual or distributed conflicts in program logic.

For example: Say you change the arguments to function X defined in file A. At the same time, someone edits file B, adding new calls to function X using the old arguments. You are outside the realm of **`cvs`**'s competence.

Acquire the habit of reading specs and talking to your peers.

**`cvs`** does not have change control
Change control refers to a number of things. First of all it can mean bug-tracking, that is being able to keep a database of reported bugs and the status of each one (is it fixed? in what release? has the bug submitter agreed that it is fixed?). For interfacing **`cvs`** to an external bug-tracking system, see the rcsinfo and verifymsg files (see Administrative files).
Another aspect of change control is keeping track of the fact that changes to several files were in fact changed together as one logical change. If you check in several files in a single **`cvs`** commit operation, **`cvs`** then forgets that those files were checked in together, and the fact that they have the same log message is the only thing tying them together. Keeping a gnu style ChangeLog can help somewhat.
Another aspect of change control, in some systems, is the ability to keep track of the status of each change. Some changes have been written by a developer, others have been reviewed by a second developer, and so on. Generally, the way to do this with **`cvs`** is to generate a diff (using **`cvs`** diff or diff) and email it to someone who can then apply it using the patch utility. This is very flexible, but depends on mechanisms outside **`cvs`** to make sure nothing falls through the cracks.

**`cvs`** is not an automated testing program
It should be possible to enforce mandatory use of a test suite using the commitinfo file. I haven't heard a lot about projects trying to do that or whether there are subtle gotchas, however.
**`cvs`** does not have a built-in process model
Some systems provide ways to ensure that changes or releases go through various steps, with various approvals as needed. Generally, one can accomplish this with **`cvs`** but it might be a little more work. In some cases you'll want to use the commitinfo, loginfo, rcsinfo, or verifymsg files, to require that certain steps be performed before **`cvs`** will allow a checkin. Also consider whether features such as branches and tags can be used to perform tasks such as doing work in a development tree and then merging certain changes over to a stable tree only once they have been proven.

## A Sample Session

As a way of introducing **`cvs`**, we'll go through a typical work-session using **`cvs`**. The first thing to understand is that **`cvs`** stores all files in a centralized repository (see Repository); this section assumes that a repository is set up.
Suppose you are working on a simple compiler. The source consists of a handful of C files and a Makefile. The compiler is called `tc` (Trivial Compiler), and the repository is set up so that there is a module called `tc`.

Getting the source: Creating a workspace
Committing your changes: Making your work available to others
Cleaning up: Cleaning up
Viewing differences: Viewing differences

### Getting the source

The first thing you must do is to get your own working copy of the source for `tc'. For this, you use the checkout command:

     $ cvs checkout tc

This will create a new directory called tc and populate it with the source files.

     $ cd tc
     $ ls
     **`cvs`**         Makefile    backend.c   driver.c    frontend.c  parser.c

The **`cvs`** directory is used internally by **`cvs`**. Normally, you should not modify or remove any of the files in it.

You start your favorite editor, hack away at backend.c, and a couple of hours later you have added an optimization pass to the compiler. A note to rcs and sccs users: There is no need to lock the files that you want to edit. See Multiple developers, for an explanation.
Next: Cleaning up, Previous: Getting the source, Up: A sample session

### Committing your changes

When you have checked that the compiler is still compilable you decide to make a new version of `backend.c`. This will store your new `backend.c` in the repository and make it available to anyone else who is using that same repository.

```bash
$ cvs commit backend.c
```

**`cvs`** starts an editor, to allow you to enter a log message. You type in `"Added an optimization pass."`, save the temporary file, and exit the editor.

The environment variable `$CVSEDITOR` determines which editor is started. If `$CVSEDITOR` is not set, then if the environment variable `$EDITOR` is set, it will be used. If both `$CVSEDITOR` and `$EDITOR` are not set then there is a default which will vary with your operating system, for example `vi` for unix or `notepad` for Windows NT/95.

In addition, **`cvs`** checks the `$VISUAL` environment variable. Opinions vary on whether this behavior is desirable and whether future releases of **`cvs`** should check `$VISUAL` or ignore it. You will be OK either way if you make sure that `$VISUAL` is either unset or set to the same thing as `$EDITOR`.
When **`cvs`** starts the editor, it includes a list of files which are modified. For the **`cvs`** client, this list is based on comparing the modification time of the file against the modification time that the file had when it was last gotten or updated. Therefore, if a file's modification time has changed but its contents have not, it will show up as modified. The simplest way to handle this is simply not to worry about it—if you proceed with the commit **`cvs`** will detect that the contents are not modified and treat it as an unmodified file. The next `update` will clue **`cvs`** in to the fact that the file is unmodified, and it will reset its stored timestamp so that the file will not show up in future editor sessions.
If you want to avoid starting an editor you can specify the log message on the command line using the `-m` flag instead, like this:

```bash
$ cvs commit -m "Added an optimization pass" backend.c
```

Next: Viewing differences, Previous: Committing your changes, Up: A sample session

### Cleaning up

Before you turn to other tasks you decide to remove your working copy of tc. One acceptable way to do that is of course

```bash
$ cd ..
$ rm -r tc
```

but a better way is to use the release command (see [release]()):

```bash
$ cd ..
$ cvs release -d tc
M driver.c
? tc
You have [1] altered files in this repository.
Are you sure you want to release (and delete) directory 'tc': n
** 'release' aborted by user choice.
```

The `release` command checks that all your modifications have been committed. If history logging is enabled it also makes a note in the history file. See [history file]().

When you use the `-d` flag with `release`, it also removes your working copy.

In the example above, the `release` command wrote a couple of lines of output. `? tc` means that the file `tc` is unknown to **`cvs`**. That is nothing to worry about: `tc` is the executable compiler, and it should not be stored in the repository. See [cvsignore](), for information about how to make that warning go away. See [release output](), for a complete explanation of all possible output from `release`.

`M driver.c` is more serious. It means that the file `driver.c` has been modified since it was checked out.

The `release` command always finishes by telling you how many modified files you have in your working copy of the sources, and then asks you for confirmation before deleting any files or making any note in the history file.

You decide to play it safe and answer `n <RET>` when `release` asks for confirmation.

### Viewing differences

You do not remember modifying driver.c, so you want to see what has happened to that file.

```bash
$ cd tc
$ cvs diff driver.c
```

This command runs `diff` to compare the version of `driver.c` that you checked out with your working copy. When you see the output you remember that you added a command line option that enabled the optimization pass. You check it in, and release the module.

```bash
$ cvs commit -m "Added an optimization pass" driver.c
Checking in driver.c;
/usr/local/cvsroot/tc/driver.c,v  <--  driver.c
new revision: 1.2; previous revision: 1.1
done
$ cd ..
$ cvs release -d tc
? tc
You have [0] altered files in this repository.
Are you sure you want to release (and delete) directory `tc': y
```
