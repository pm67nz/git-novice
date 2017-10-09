---
title: Ignoring Things
teaching: 5
exercises: 0
questions:
- "How can I tell Pijul to ignore files I don't want to track?"
objectives:
- "Configure Pijul to ignore specific files."
- "Explain why ignoring files can be useful."
keypoints:
- "The `.ignore` file tells Pijul what files to ignore."
---

What if we have files that we do not want Pijul to track for us,
like backup files created by our editor
or intermediate files created during data analysis.
Let's create a few dummy files:

~~~
$ mkdir results
$ touch a.dat b.dat c.dat results/a.out results/b.out
~~~
{: .bash}

and see what Pijul says:

~~~
$ pijul status
~~~
{: .bash}

~~~
Untracked files:
  (use "pijul add <file>..." to track them)

        a.dat
        b.dat
        c.dat
        results
        results/a.out
        results/b.out
~~~
{: .output}

Putting these files under version control would be a waste of disk space.
What's worse,
having them all listed could distract us from changes that actually matter,
so let's tell Pijul to ignore them.

We do this by creating a file in the root directory of our project called `.ignore`:

~~~
$ nano .ignore
$ cat .ignore
~~~
{: .bash}

~~~
*.dat
results
~~~
{: .output}

These patterns tell Pijul to ignore any file whose name ends in `.dat`
and everything in the `results` directory.
(If any of these files were already being tracked,
Pijul would continue to track them.)

Once we have created this file,
the output of `pijul status` is much cleaner:

~~~
$ pijul status
~~~
{: .bash}

~~~
Untracked files:
  (use "pijul add <file>..." to track them)

        .ignore
~~~
{: .output}

The only thing Pijul notices now is the newly-created `.ignore` file.
You might think we wouldn't want to track it,
but everyone we're sharing our repository with will probably want to ignore
the same things that we're ignoring.
Let's add and commit `.ignore`:

~~~
$ pijul add .ignore
$ pijul record -m "Add the ignore file"
$ pijul status
~~~
{: .bash}

~~~
Nothing to record, working tree clean
~~~
{: .output}

If you really don't want to share your .ignore patterns then you can put them into 
`.pijul/local/.ignore` instead.  That file works the same way except that
 it never gets tracked.
 
