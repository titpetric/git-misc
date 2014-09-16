git-robot
=========

Checkout assistant for running continous deployment checkouts.

Needs `git-robot*.json` under the working path (can have many of them).

```
{
  "last_modified": "file_or_url_with_last_modified_info.json",
  "checkout": [
    {
      "repository": "git@github.com:titpetric/git-misc.git",
      "folder": "optional/output/folder/in/lieu/of/default"
    },
    ...
```

It is possible to use a "template" approach, if your modules follow
the same naming scheme. This cuts down the verbosity of the checkout
entries in the configuration files. The application will register
one checkout for each entry in the `name` array, replacing the variable
`{name}` in the relevant `repository` and `folder` entries. See this example:

```
{
  "last_modified": "file_or_url_with_last_modified_info.json",
  "checkout": [
    {
      "name": ["news", "articles", "faq", "admin"],
      "repository": "git@github.com:titpetric/cms-page-{name}.git",
      "folder": "pages/{name}"
    },
    ...
```

`file_or_url_with_last_modified_info.json` should contain the time of
the last push to the repository. You are free to implement this as you
wish, the main point is that this timestamp should change every time
you want to automatically check out anything with git-robot:

```
{
"/titpetric/git-misc/": "2013-08-15 13:13:13",
...
}
```

If the modification timestamp for the repository is not listed, git-robot
will check out / pull the repository once every 24 hours and not before.

You can run git-robot via crontab. It will produce output only when there
is something to check out (when your contents modify). Keep in mind you
have to add a SSH key for deployment to the repositories you want to check
out via git robot used with crontab.

If you want to force update of all your repositories, go to your project
checkout path and run `git robot update`.

TODO:

1. configure timeout for non-tracked repositories
2. provide example hook for github/bitbucket to track last modified data
3. improve documentation

Sorry, pretty badly documented so far, send me a message if you need help.


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
# ./git-st
 M README.md
?? git-st
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
# ./git-misc
 M README.md
?? git-up
# ./minitpl
 M git_hooks
# ./research-projects
 M git_hooks
```

It seems `git_hooks` was previously out of date on `minitpl` and `research-projects`.

Install
-------

Copy the files somewhere to your PATH, like `/usr/local/bin`.
Run `git st` and `git up` respectively.