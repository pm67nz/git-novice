---
title: Tracking Changes
teaching: 20
exercises: 0
questions:
- "How do I record changes in Pijul?"
- "How do I check the status of my version control repository?"
- "How do I record notes about what changes I made and why?"
objectives:
- "Go through the modify-record cycle for one or more files."
- "Explain where information is stored at each stage of the Pijul workflow."
- "Distinguish between descriptive and non-descriptive patch descriptions."
keypoints:
- "`pijul status` shows the status of a repository."
- "Files can be stored in a project's working directory (which users see), and the local repository (where commits are permanently recorded)."
- "`pijul add` tells Pijul to start tracking files."
- "`pijul record` saves any changes to the tracked files as a new patch in the local repository."
- "Always write a log message when recording patches."
---

First let's make sure we're still in the right directory.
You should be in the `planets` directory.

~~~
$ pwd
~~~
{: .bash}

If you are still in `moons` navigate back up to `planets`

~~~
$ cd ..
~~~
{: .bash}

Let's create a file called `mars.txt` that contains some notes
about the Red Planet's suitability as a base.
We'll use `nano` to edit the file;
you can use whatever editor you like.
The bash command to create or edit a new file will depend on the editor you choose (it might not be `nano`). For a refresher on text editors, check out ["Which Editor?"](https://swcarpentry.github.io/shell-novice/03-create/) in [The Unix Shell](https://swcarpentry.github.io/shell-novice/) lesson.

~~~
$ nano mars.txt
~~~
{: .bash}

Type the text below into the `mars.txt` file:

~~~
Cold and dry, but everything is my favorite color
~~~
{: .output}

`mars.txt` now contains a single line, which we can see by running:

~~~
$ ls
~~~
{: .bash}

~~~
mars.txt
~~~
{: .output}

~~~
$ cat mars.txt
~~~
{: .bash}

~~~
Cold and dry, but everything is my favorite color
~~~
{: .output}

If we check the status of our project again,
Pijul tells us that it's noticed the new file:

~~~
$ pijul status
~~~
{: .bash}

~~~
Untracked files:
  (use "pijul add <file>..." to track them)

        mars.txt
~~~
{: .output}

The "untracked files" message means that there's a file in the directory
that Pijul isn't keeping track of.
We can tell Pijul to track a file using `pijul add`:

~~~
$ pijul add mars.txt
~~~
{: .bash}

and then check that the right thing happened:

~~~
$ pijul status
~~~
{: .bash}

~~~
Changes not yet recorded:
  (use "pijul record ..." to record a new patch)

        new file:  mars.txt
~~~
{: .output}

Pijul now knows that it's supposed to keep track of `mars.txt`,
but it hasn't recorded it in the repository yet.
To get it to do that, we need to run one more command:

~~~
$ pijul record
~~~
{: .bash}

~~~
added file /Users/pmax001/tmp/planets/mars.txt

Shall I record this change? (1/2) [ynkad] 
~~~
{: .output}

Tell it `y` for "Yes"

~~~
In file "/Users/pmax001/tmp/planets/mars.txt"

+ Cold and dry, but everything is my favorite color

Shall I record this change? (2/2) [ynkad] 
~~~

Again, `y` for "Yes".

~~~
What is the name of this patch?
~~~

Give a short, descriptive, and specific comment that will help us remember later on what we did and why, 
for example: "Start notes on Mars as a base".

~~~
What is your name <and email address>? 
~~~

Because this is the first time you have added content to a repository Pijul has asked you for your email address. This address will be associated with this change and your future ones, which will become important when you share your changes with other people.  

~~~
Recorded patch AScIP6rhgj4vq996IEpxO88CAV9uT-R3mLzhCSIt6SEgDa5Ebn1phl0xLMubjme-l-YrTFdZ-j7Xqff8gwKqaS8
~~~

When we run `pijul record`,
Pijul takes everything which has changed in the tracked files 
and stores that permanently inside the special `.pijul` directory.
This permanent copy is called a [patch]({{ page.root }}/reference/#patch)
and its "hash" which serves as a permanent internal identifier is "AScIP6rhgj4vq996IEpxO88CAV9uT-R3mLzhCSIt6SEgDa5Ebn1phl0xLMubjme-l-YrTFdZ-j7Xqff8gwKqaS8". Your patch may have a different hash.

You can use the `-m` option of `pijul record` (for "message")
to specify the patch name on the command line instead of being prompted for it.

[Good patch names][patch-names] are a brief (<50 characters) summary of
changes made in the patch.

If we run `pijul status` now:

~~~
$ pijul status
~~~
{: .bash}

~~~
Nothing to record, working tree clean
~~~
{: .output}

it tells us everything is up to date.
If we want to know what we've done recently,
we can ask pijul to show us the project's history using `pijul changes`:

~~~
$ pijul changes
~~~
{: .bash}

~~~
Hash: AbJ7yztMWV-Hk0tw3Jvds88J4RohykTCzN-hP87oTiPRXfp7k_yfjCOhqyKk26fKmhYp9tHvL4Kz8zJoJupXTuE
Internal id: snvLO0xZX4c
Authors: ["Vlad Dracula <vlad@tran.sylvan.ia>"]
Timestamp: 2017-09-28 10:10:43.549970 UTC

    Start notes on Mars as a base
~~~
{: .output}

`pijul changes` lists all patches recorded in the repository.
The listing for each patch includes
its author, when it was created,
and the name it was given when it was created.

> ## Where Are My Changes?
>
> If we run `ls` at this point, we will still see just one file called `mars.txt`.
> That's because Pijul saves information about files' history
> in the special `.pijul` directory mentioned earlier
> so that our filesystem doesn't become cluttered
> (and so that we can't accidentally edit or delete an old version).
{: .callout}

Now suppose Dracula adds more information to the file.
(Again, we'll edit with `nano` and then `cat` the file to show its contents;
you may use a different editor, and don't need to `cat`.)

~~~
$ nano mars.txt
$ cat mars.txt
~~~
{: .bash}

~~~
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
~~~
{: .output}

When we run `pijul status` now,
it tells us that a file it already knows about has been modified:

~~~
$ pijul status
~~~
{: .bash}

~~~
Changes not yet recorded:
  (use "pijul record ..." to record a new patch)

        modified:  mars.txt
~~~
{: .output}

It is good practice to review
our changes before saving them. We do this using `pijul diff`.
This shows us the differences between the current state
of the file and what is recorded in the repository:

~~~
$ pijul diff
~~~
{: .bash}

~~~
In file "~/dracula/planets/mars.txt"

+ The two moons may be a problem for Wolfman
~~~
{: .output}

After reviewing our change, it's time to record it:

~~~
$ pijul record -m "Add concerns about effects of Mars' moons on Wolfman"
$ pijul status
~~~
{: .bash}

~~~
Nothing to record, working tree clean
~~~
{: .output}


Let's add another line to the file:

~~~
$ nano mars.txt
$ cat mars.txt
~~~
{: .bash}

~~~
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
~~~
{: .output}

~~~
$ pijul record -m "Discuss concerns about Mars' climate for Mummy"
~~~
{: .bash}

and look at the history of what we've done so far:

~~~
$ pijul changes
~~~
{: .bash}

~~~
Hash: AUfGUxyLI9-kSLLa7UpuG5xWhsoHXRQTXsnSNSQbunK-qUgaSGSplG3YhTWb1Tpi-Rdr2mREeYf8Ap6Xgg_HOB8
Internal id: R8ZTHIsj36Q
Authors: ["Vlad Dracula <vlad@tran.sylvan.ia>"]
Timestamp: 2017-09-28 10:25:29.271196 UTC

    Discuss concerns about Mars' climate for Mummy

Hash: AUI2zumJX7xHr27JErfJIJauE3_KEfKGn3CS1asWMrPpADIZLw_uQW_R0w80QJo6n3k77cVlGXBLwp6qpiqwil4
Internal id: QjbO6YlfvEc
Authors: ["Vlad Dracula <vlad@tran.sylvan.ia>"]
Timestamp: 2017-09-28 10:21:56.863250 UTC

    Add concerns about effects of Mars' moons on Wolfman

Hash: AbJ7yztMWV-Hk0tw3Jvds88J4RohykTCzN-hP87oTiPRXfp7k_yfjCOhqyKk26fKmhYp9tHvL4Kz8zJoJupXTuE
Internal id: snvLO0xZX4c
Authors: ["Vlad Dracula <vlad@tran.sylvan.ia>"]
Timestamp: 2017-09-28 10:10:43.549970 UTC

    Start notes on Mars as a base
~~~
{: .output}

> ## Paging the output
>
> When the output of `pijul changes` is too long to fit in your screen,
> `pijul` uses a program to split it into pages of the size of your screen.
> When this "pager" is called, you will notice that the last line in your
> screen is a `:`, instead of your usual prompt.
>
> *   To get out of the pager, press `q`.
> *   To move to the next page, press the space bar.
> *   To search for `some_word` in all pages, type `/some_word`
>     and navigate through matches pressing `n`.
{: .callout}

> ## Directories
>
> If you create a directory in your Git repository and populate it with files,
> you can add all files in the directory at once by:
>
> ~~~
> pijul add directory_name --recursive 
> ~~~
> {: .bash}
>
{: .callout}


