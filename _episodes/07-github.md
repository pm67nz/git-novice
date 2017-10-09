---
title: Remote Repositories in the Nest
teaching: 30
exercises: 0
questions:
- "How do I share my changes with others on the web?"
objectives:
- "Explain what remote repositories are and why they are useful."
- "Push to or pull from a remote repository."
keypoints:
- "A local Pijul repository can be connected to one or more remote repositories."
- "Use the HTTPS protocol to connect to remote repositories until you have learned how to set up SSH."
- "`pijul push` copies changes from a local repository to a remote repository."
- "`pijul pull` copies changes from a remote repository to a local repository."
---

Version control really comes into its own when we begin to collaborate with
other people.  We already have most of the machinery we need to do this; the
only thing missing is to copy changes from one repository to another.

Systems like Pijul allow us to move work between any two repositories.  That can be 
done directly between people's personal repositories, but when the team of contributors gets large
it becomes easiest to use one copy as a central hub, and to keep it
on the web rather than on someone's laptop.  For Pijul there is a hosting service called 
[The Nest](http://nest.pijul.com) to hold those master copies.  For other distributed version control systems there
is [GitHub](http:github.com), [BitBucket](http://bitbucket.org) or
[GitLab](http://gitlab.com/).

Let's start by sharing the changes we've made to our current project with the
world.  Create an account in the Nest. 

At the end of your Nest profile page give `planets` as the name of your new repository and click the
"Add repository" button.

This effectively does the following on the Nest's servers:

~~~
$ mkdir planets
$ cd planets
$ pijul init
~~~
{: .bash}

Our local repository still contains our earlier work on `mars.txt`, but the
remote repository on the Nest doesn't contain any files yet, as you will see when you
follow the newly displayed link to the repository's page.

After taking in the lack of files, switch to the "admin" tab so you can configure the repository.

Now we need to set things up so that the Nest knows who you are when you talk to it directly from the command line:

~~~
$ pijul keys --generate-ssh
$ pijul keys --generate-signing
$ pijul keys --upload-to vlad@nest.pijul.com
~~~
{: .bash}

Go into the local `planets` repository, and run this command:

~~~
$ pijul push vlad@nest.pijul.com/vlad/planets
~~~
{: .bash}

Make sure to use the URL for your repository rather than Vlad's: the only
difference should be your username instead of `vlad`.

~~~
~~~
{: .output}

We can pull changes from the remote repository to the local one as well:

~~~
$ pijul pull
~~~
{: .bash}

~~~
~~~
{: .output}

Pulling has no effect in this case because the two repositories are already
synchronized.  If someone else had pushed some changes to the repository on
the Nest, though, this command would download them to our local repository.

> ## Push vs. Record
>
> In this lesson, we introduced the "pijul push" command.
> How is "pijul push" different from "pijul record"?
>
> > ## Solution
> > When we push changes, we're interacting with a remote repository to update it with the changes we've made locally (often this corresponds to sharing the changes we've made with others). Record only updates your local repository.
> {: .solution}
{: .challenge}

