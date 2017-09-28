---
title: Creating a Repository
teaching: 10
exercises: 0
questions:
- "Where does Pijul store information?"
objectives:
- "Create a local Pijul repository."
keypoints:
- "`pijul init` initializes a repository."
---

Once Pijul is installed, we can start using it.  On a command line, Pijul commands are written as `pijul command`,
where `command` is what we actually want to do. 

> ## Pijul Help and Manual
>
> If you forget a `pijul` command, you can access the list of commands by using `pijul help`, and get help on
> any specific command with `--help`:
> ~~~
> $ pijul help
> ~~~
> {: .bash}
> 
> ~~~
> pijul 0.8.0
> Pierre-Étienne Meunier and Florent Becker
> Version Control: fast, distributed, easy to use; pick any three
> 
> USAGE:
>     pijul [SUBCOMMAND]
> 
> FLAGS:
>     -h, --help       Prints help information
>     -V, --version    Prints version information
> 
> SUBCOMMANDS:
>     add                  add a file to the repository
>     apply                apply a patch
>     blame                Show what patch introduced each line of a file.
>     branches             List all branches
>     changes              List the patches applied to the given branch
>     checkout             Change the current branch
>     clone                clone a remote branch
>     delete-branch        Delete a branch in the local repository
>     diff                 show what would be recorded if record were called
>     dist                 Produces a tar.gz archive of the repository
>     fork                 Create a new branch
>     help                 Prints this message or the help of the given subcommand(s)
>     init                 Create a new repository
>     keys                 Manage signing and SSH keys
>     ls                   list tracked files
>     mv                   Change file names
>     patch                Output a patch (in binary)
>     pull                 pull from a remote repository
>     push                 push to a remote repository
>     record               record changes in the repository
>     remove               remove file from the repository
>     revert               Rewrite the working copy from the pristine
>     show-dependencies    Print the patch dependencies using the DOT syntax in stdout
>     status               Show working tree status
>     unrecord             Unrecord some patches (remove them without reverting them)
> ~~~
> {: .output}
>
> The first subcommand we are going to use is `init`:
> ~~~
> $ pijul init --help
> ~~~
> {: .bash}
> 
> ~~~
> pijul-init 
> Create a new repository
> 
> USAGE:
>     pijul init [directory]
> 
> FLAGS:
>     -h, --help       Prints help information
>     -V, --version    Prints version information
> 
> ARGS:
>     <directory>    Where to create the repository, defaults to the current repository.
> ~~~
> {: .output}
{: .callout}


Let's create a directory for our work and then move into that directory:

~~~
$ mkdir planets
$ cd planets
~~~
{: .bash}

Then we tell Pijul to make `planets` into a [repository]({{ page.root }}/reference/#repository)—a place where
Pijul can store versions of our files. :

~~~
$ pijul init
~~~
{: .bash}

If we use `ls` to show the directory's contents,
it appears that nothing has changed:

~~~
$ ls
~~~
{: .bash}

But if we add the `-a` flag to show everything,
we can see that Pijul has created a hidden directory within `planets` called `.pijul`:

~~~
$ ls -a
~~~
{: .bash}

~~~
.	..	.pijul
~~~
{: .output}

Pijul stores information about the project in this special sub-directory.
If we ever delete it, we will lose the project's history.

We can check that everything is set up correctly
by asking Pijul to tell us the status of our project:

~~~
$ pijul status
~~~
{: .bash}

~~~
Nothing to record, working tree clean
~~~
{: .output}

> ## Places to Create Pijul Repositories
>
> Dracula starts a new project, `moons`, related to his `planets` project.
> Despite Wolfman's concerns, he enters the following sequence of commands to
> create one Pijul repository inside another:
>
> ~~~
> $ cd             # return to home directory
> $ mkdir planets  # make a new directory planets
> $ cd planets     # go into planets
> $ pijul init     # make the planets directory a Git repository
> $ mkdir moons    # make a sub-directory planets/moons
> $ cd moons       # go into planets/moons
> $ pijul init     # make the moons sub-directory a Git repository
> Repository /home/dracula/planets already exists
> ~~~
> {: .bash}
>
> Why does Pijul not let Dracula do this? 
> > Pijul repositories can interfere with each other if they are "nested" in the
> > directory of another: the outer repository will try to version-control 
> > the inner repository. Therefore, each new Pijul
> > repository must be created in a separate directory. 
> >
> > Note that we can track files in directories within a repository:
> >
> > ~~~
> > $ touch moon phobos deimos titan      # create moon files
> > $ cd ..                               # return to planets directory
> > $ ls moons                            # list contents of the moons directory
> > $ pijul add moons/*                   # add all contents of ./planets/moons
> > $ pijul status                        # show that the moons files are being tracked by Pijul
> > $ pijul record -m "add moon files"    # commit ./planets/moons to planets repository
> > ~~~
> > {: .bash}
> >
> > Similarly, we can ignore (as discussed later) entire directories, such as the `moons` directory:
> >
> > ~~~
> > $ nano .pijul/local/ignore # open the ignore file in the text editor to add the moons directory
> > $ cat .pijul/local/ignore # if you run cat afterwards, it should look like this:
> > ~~~
> > {: .bash}
> >
> {: .solution}
{: .challenge}
