****************
Unix Cheatsheet
****************

:title: Unix Cheatsheet
:date: 2020-12-07
:category: Posts
:slug: unix-cheatsheet
:tags: unix

Here is a small set of useful ``unix`` utilities, scripts and one-liners.

Example grep command w/recursive, line numbers, ignore case, and exclude-dirs:
##############################################################################
::

    grep -nri clue . --exclude-dir={node_modules,other_dir}

Example grep command for strings with "-" in them:
##################################################
::

    grep -nri -- "-X" .
