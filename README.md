git-misc
========

I am working with many checkouts and submodules. It has become tedious to track
modifications in all of these folders. I created wrappers around `git status`
to display modifications under a directory tree. This is `git-st`.

Often I need to update several checkouts to their current state from a remote
git. I created `git-up`, to issue a `git pull` command on every found repository.


git-st
------

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

```
ages:~/github# git up
# ./git_hooks
Already up-to-date.
# ./git-misc
Already up-to-date.
# ./minitpl/git_hooks
remote: Counting objects: 11, done.
remote: Compressing objects: 100% (5/5), done.
remote: Total 7 (delta 2), reused 7 (delta 2)
Unpacking objects: 100% (7/7), done.
From github.com:titpetric/git_hooks
   b179ef1..2deb74e  master     -> origin/master
Updating b179ef1..2deb74e
Fast-forward
 README.md          |    6 +++---
 pre-commit/phpunit |   12 ------------
 2 files changed, 3 insertions(+), 15 deletions(-)
# ./minitpl
Already up-to-date.
# ./composer-sentinel
Already up-to-date.
# ./research-projects/git_hooks
remote: Counting objects: 22, done.
remote: Compressing objects: 100% (15/15), done.
remote: Total 18 (delta 3), reused 18 (delta 3)
Unpacking objects: 100% (18/18), done.
From github.com:titpetric/git_hooks
   3f69148..2deb74e  master     -> origin/master
Updating 3f69148..2deb74e
Fast-forward
 README.md            |    6 +-
 pre-commit/js-lint   |   42 --------------------
 pre-commit/json-lint |   48 -----------------------
 pre-commit/lint      |  103 ++++++++++++++++++++++++++++++++++++++++++++++++++
 pre-commit/php-lint  |   37 ------------------
 pre-commit/phpunit   |   12 ------
 pre-commit/xml-lint  |   56 ---------------------------
 7 files changed, 106 insertions(+), 198 deletions(-)
 delete mode 100755 pre-commit/js-lint
 delete mode 100755 pre-commit/json-lint
 create mode 100755 pre-commit/lint
 delete mode 100755 pre-commit/php-lint
 delete mode 100755 pre-commit/xml-lint
# ./research-projects
Already up-to-date.
# ./git-st
Already up-to-date.
# ./git-hooks
Already up-to-date.
```

My whole checked out projects folder is now up to date. Since I also
modified some submodules with this command (git_hooks), I need to
commit these. To see what changed over the whole tree, I issue a `git st`:

```
ages:~/github# git st
# ./git_hooks
# ./git-misc
 M README.md
?? .#README.md
?? git-up
# ./minitpl/git_hooks
# ./minitpl
 M git_hooks
# ./composer-sentinel
# ./research-projects/git_hooks
# ./research-projects
 M git_hooks
# ./git-st
# ./git-hooks
```

It seems `git_hooks` was previously out of date on `minitpl` and `research-projects`.

Install
-------

Copy the files somewhere to your PATH, like `/usr/local/bin`.
Run `git st` and `git up` respectively.