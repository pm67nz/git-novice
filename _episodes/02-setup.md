---
title: Setting Up Pijul
teaching: 5
exercises: 0
questions:
- "How do I get set up to use Pijul?"
objectives:
- "Configure `pijul` the first time it is used on a computer."
- "Understand how to customize your pijul configuration."
keypoints:
-   "Change your email address, editor, and other preferences once per machine."
---

When you used Pijul for the first time, it asked you for your email address.  
If you later need to change this address you will need to edit the file
`~/.pijulconfig/config`

~~~
nano ~/.pijulconfig/config
~~~

In the future it will also be possible to set your prefered editor here for editing
patch descriptions.

For these lessons, we will be interacting with the [Nest](http://nest.pijul.com/) and so the email address used should be the same as the one used when setting up your Nest account. 
If you elect to use a private email address with the Nest, then use that same email address for pijul.

Dracula also has to set his favorite text editor, following this table:

| Editor             | Configuration command                            |
|:-------------------|:-------------------------------------------------|
| Atom | `$ git config --global core.editor "atom --wait"`|
| nano               | `$ git config --global core.editor "nano -w"`    |
| Text Wrangler (Mac)      | `$ git config --global core.editor "edit -w"`    |
| Sublime Text (Mac) | `$ git config --global core.editor "subl -n -w"` |
| Sublime Text (Win, 32-bit install) | `$ git config --global core.editor "'c:/program files (x86)/sublime text 3/sublime_text.exe' -w"` |
| Sublime Text (Win, 64-bit install) | `$ git config --global core.editor "'c:/program files/sublime text 3/sublime_text.exe' -w"` |
| Notepad++ (Win, 32-bit install)    | `$ git config --global core.editor "'c:/program files (x86)/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"`|
| Notepad++ (Win, 64-bit install)    | `$ git config --global core.editor "'c:/program files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"`|
| Kate (Linux)       | `$ git config --global core.editor "kate"`       |
| Gedit (Linux)      | `$ git config --global core.editor "gedit --wait --new-window"`   |
| Scratch (Linux)       | `$ git config --global core.editor "scratch-text-editor"`  |
| emacs              | `$ git config --global core.editor "emacs"`   |
| vim                | `$ git config --global core.editor "vim"`   |

It is possible to reconfigure the text editor for Git whenever you want to change it.

> ## Exiting Vim
>
> Note that `vim` is the default editor for for many programs, if you haven't used `vim` before and wish to exit a session, type `Esc` then `:q!` and `Enter`.
{: .callout}


> ## Pijul Help and Manual
>
> Always remember that if you forget a `pijul` command, you can access the list of commands by using `pijul help`, and get help on
> any specific command with `--help`:
>
> ~~~
> $ pijul help
> $ pijul init --help
> ~~~
> {: .bash}
{: .callout}

Online documentation for Pijul can be found at the [Pijul web site](https://pijul.org/manual/)
