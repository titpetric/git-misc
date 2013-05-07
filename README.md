git-st
======

Enhanced git status shorthand, out of tree git status

What does it do?
----------------

If you're working with many checkouts or submodules, it can sometimes be tedious
to track modifications in each one. This script is a wrapper around `git status`
which provides status for a whole directory tree.

Running `git st` will give you output like this:

```
$ git st
# ./git_hooks
# ./minitpl/git_hooks
# ./minitpl
# ./composer-sentinel
# ./research-projects/git_hooks
# ./research-projects
# ./git-st
 M README.md
?? git-st
# ./git-hooks
```

The example lists my current github projects folder, with several cloned projects
and a few submodules (git_hooks). I am running `git st` outside of any git folder.

Possible improvements in the future (changed output):
```
$ git st
 M ./git-st/README.md
?? ./git-st/git-st
```

The goal of the script is to keep the `git status` output down to a minimum,
provide a shorthand command `git st` and display status for all your git
project trees under your current folder.

Usage
-----

Copy the file `git-st` somewhere to your PATH, like `/usr/local/bin`.

Run `git st` in your repository.