---
title: Exploring History
teaching: 25
exercises: 0
questions:
- "How can I recover old versions of files?"
objectives:
- "Identify and use patch names."
- "Undo changes made to files."
keypoints:
- "`pijul unrecord` undoes changes."
---

All right! So
we can save changes to files and see what we've changed—now how
can we restore older versions of things?
Let's suppose we accidentally overwrite our file:

~~~
$ nano mars.txt
$ cat mars.txt
~~~
{: .bash}

~~~
We will need to manufacture our own oxygen
~~~
{: .output}

We can put things back the way they were
by using `pijul revert`:

~~~
$ pijul revert
~~~
{: .bash}

~~~
In file "~dracula/planets/mars.txt"

- Cold and dry, but everything is my favorite color
- The two moons may be a problem for Wolfman
- But the Mummy will appreciate the lack of humidity
+ We will need to manufacture our own oxygen

Shall I revert this change? (1/1) [ynkad] 
~~~
{: .output}

As usual, answer "y" for Yes.  The other options are No, sKip, All, and Done.

~~~
$ cat mars.txt
~~~
{: .bash}

~~~
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
~~~
{: .output}

`pijul reset` puts everything back as it should be according to the repository, in 
other words it discards any changes which have not yet been recorded.

But what if we have already saved the change?, then we will have to remove the 
the saved changes from the repository:

Let's make another change to `mars.txt`.

~~~
$ nano mars.txt
$ pijul diff
~~~
{: .bash}

~~~
In file "~dracula/planets/mars.txt"

+ An ill-considered change

~~~
{: .output}

Now, we record that change:

~~~
$ pijul record -a -m "I am going to regret this"
$ pijul changes
~~~
{: .bash}

~~~
Hash: AfoEEkyla6Xr5OORUDeMCgOba1_dghEERjmYOcKf1JfpoahEL7RlhlwZWbY3ghi5UlS9425DPSYeN0rjbEz22_U
Internal id: -gQSTKVrpes
Authors: [""Vlad Dracula <vlad@tran.sylvan.ia>""]
Timestamp: 2017-09-29 05:17:07.894942 UTC

    I am going to regret this

Hash: AUfGUxyLI9-kSLLa7UpuG5xWhsoHXRQTXsnSNSQbunK-qUgaSGSplG3YhTWb1Tpi-Rdr2mREeYf8Ap6Xgg_HOB8
Internal id: R8ZTHIsj36Q
Authors: [""Vlad Dracula <vlad@tran.sylvan.ia>""]
Timestamp: 2017-09-28 10:25:29.271196 UTC

    Discuss concerns about Mars' climate for Mummy

Hash: AUI2zumJX7xHr27JErfJIJauE3_KEfKGn3CS1asWMrPpADIZLw_uQW_R0w80QJo6n3k77cVlGXBLwp6qpiqwil4
Internal id: QjbO6YlfvEc
Authors: [""Vlad Dracula <vlad@tran.sylvan.ia>""]
Timestamp: 2017-09-28 10:21:56.863250 UTC

    Add concerns about effects of Mars' moons on Wolfman

Hash: AbJ7yztMWV-Hk0tw3Jvds88J4RohykTCzN-hP87oTiPRXfp7k_yfjCOhqyKk26fKmhYp9tHvL4Kz8zJoJupXTuE
Internal id: snvLO0xZX4c
Authors: [""Vlad Dracula <vlad@tran.sylvan.ia>""]
Timestamp: 2017-09-28 10:10:43.549970 UTC

    Start notes on Mars as a base
~~~
{: .output}


~~~
$ piujl unrecord
~~~
{: .bash}

~~~
Hash: AfoEEkyla6Xr5OORUDeMCgOba1_dghEERjmYOcKf1JfpoahEL7RlhlwZWbY3ghi5UlS9425DPSYeN0rjbEz22_U
Internal id: -gQSTKVrpes
Authors: [""Vlad Dracula <vlad@tran.sylvan.ia>""]
Timestamp: 2017-09-29 05:17:07.894942 UTC

    I am going to regret this

Shall I unrecord this patch? [ynkad] 
~~~
{: .output}

Answer "y" for "Yes".

~~~
Hash: AfoEEkyla6Xr5OORUDeMCgOba1_dghEERjmYOcKf1JfpoahEL7RlhlwZWbY3ghi5UlS9425DPSYeN0rjbEz22_U
Internal id: -gQSTKVrpes
Authors: [""Vlad Dracula <vlad@tran.sylvan.ia>""]
Timestamp: 2017-09-29 05:17:07.894942 UTC

    I am going to regret this

Shall I unrecord this patch? [ynkad] 
~~~
{: .output}

We want to keep this patch and all the remaining patches, so answer "d" for "Done".

~~~
$ pijul changes
~~~
{: .bash}

~~~
Hash: AUfGUxyLI9-kSLLa7UpuG5xWhsoHXRQTXsnSNSQbunK-qUgaSGSplG3YhTWb1Tpi-Rdr2mREeYf8Ap6Xgg_HOB8
Internal id: R8ZTHIsj36Q
Authors: [""Vlad Dracula <vlad@tran.sylvan.ia>""]
Timestamp: 2017-09-28 10:25:29.271196 UTC

    Discuss concerns about Mars' climate for Mummy

Hash: AUI2zumJX7xHr27JErfJIJauE3_KEfKGn3CS1asWMrPpADIZLw_uQW_R0w80QJo6n3k77cVlGXBLwp6qpiqwil4
Internal id: QjbO6YlfvEc
Authors: [""Vlad Dracula <vlad@tran.sylvan.ia>""]
Timestamp: 2017-09-28 10:21:56.863250 UTC

    Add concerns about effects of Mars' moons on Wolfman

Hash: AbJ7yztMWV-Hk0tw3Jvds88J4RohykTCzN-hP87oTiPRXfp7k_yfjCOhqyKk26fKmhYp9tHvL4Kz8zJoJupXTuE
Internal id: snvLO0xZX4c
Authors: [""Vlad Dracula <vlad@tran.sylvan.ia>""]
Timestamp: 2017-09-28 10:10:43.549970 UTC

    Start notes on Mars as a base
~~~
{: .output}

The patch has now gone from the repository, but the change still exists in the working copy, but we can 
revert that too:

~~~
$ pijul diff
~~~
{: .bash}

~~~
In file "~dracula/planets/mars.txt"

+ An ill-considered change

~~~
{: .output}

~~~
$ pijul revert -a
$ cat mars.txt
~~~
{: .bash}

~~~
Cold and dry, but everything is my favorite color
The two moons may be a problem for Wolfman
But the Mummy will appreciate the lack of humidity
~~~
{: .output}

