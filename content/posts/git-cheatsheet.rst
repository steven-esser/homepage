**************
Git Cheatsheet
**************

:title: Git Cheatsheet
:date: 2020-09-15
:category: Posts
:slug: git-cheatsheet
:tags: git, unix

Git is a very powerful and useful tool for software development work. However, its syntax
and user interface can be confusing to anyone who is not already an expert. Below is a 
compilation of common operations and their corresponding git commands.

"Force" pull a remote branch to a local
#######################################
Sometimes you may commit to a branch (like main) by accident. Often the solution is to
reset the a branch to the state of that branch on the remote repository. To do this,
run the following:
::

    git reset --hard origin/branch

Unstage an erroneously staged file
##################################
When running ``git add``, sometimes files get staged that we may not want to stage. To 
unstage these files, run:
::

    git reset ./path/to/file/you/wish/to/unstage.txt

Revert changes on an unstaged file
##################################
If you have made changes to an unstaged file you wish to revert, run the following:
::

    git checkout ./path/to/file/you/wish/to/revert.txt

Combined unrelated git repos
############################
How to combine two separate unrelated Git repositories into one with single history
timeline:
::

    git merge remote-name branch-name --allow-unrelated-histories

Get current commit's SHA1
##########################
Getting a specific commit's SHA1 value can be useful for hyperlinking on GitHub (and other
things).
::

    git rev-parse HEAD

Make a dummy commit
###################
These dummy commits are useful for testing CI/CD and other integrations.
::

    git commit --allow-empty

Create a zip archive for a specific branch
##########################################
::

    git archive --format zip --output filename.zip master

Git tag examples
################
The following are useful ``git tag`` examples:
::

    # Show all released versions
    git tag
    
    # Show all released versions with comments
    git tag -l -n1
    
    # Create release version
    git tag v1.0.0
    
    # Create release version with comment
    git tag -a v1.0.0 -m 'Message'
    
    # Checkout a specific release version
    git checkout v1.0.0

Git log examples
################
The following are some useful ``git log`` flags:
::

  # Show one-line summary of commits
  git log --oneline
  git log --oneline -3 # show last 3
  
  # Show only custom commits
  git log --author="Steven"
  git log --grep="Message"
  git log --until=2020-01-01
  git log --since=2020-01-01
  
  # Show different summaries/stats
  git log --stat --summary
  git log --graph
  git log --oneline --graph --all --decorate

Git diff examples
#################
The following are some useful ``git diff`` flags:
::

    # Compare modified files and highlight changes only
    git diff --color-words index.html`
    
    # Compare modified files within the staging area
    git diff --staged
    
    # Compare branches
    git diff master..branchname
    git diff --color-words master..branchname^
    
    # Compare commits
    git diff 6eb715d
    git diff 6eb715d..HEAD
    git diff 6eb715d..537a09f
    
    # Compare commits of file
    git diff 6eb715d index.html
    git diff 6eb715d..537a09f index.html
    
    # Compare without caring about spaces
    git diff -b 6eb715d..HEAD
    git diff --ignore-space-change 6eb715d..HEAD
    
    # Compare without caring about all spaces
    git diff -w 6eb715d..HEAD
    git diff --ignore-all-space 6eb715d..HEAD
    
    # Useful comparings
    git diff --stat --summary 6eb715d..HEAD
    
    # Blame
    git blame -L10,+1 index.html

Split multiple subdirectories into a new git repo
#################################################
Originally from: https://stackoverflow.com/questions/2982055/detach-many-subdirectories-into-a-new-separate-git-repository

This will remove all tags as well:
::

    git filter-branch --tag-name-filter cat --index-filter 'git rm --cached -qr --ignore-unmatch -- . && git reset -q $GIT_COMMIT -- src/extractcode tests/extractcode' --prune-empty -- --all

After the above operation is finished, you will now have a repository that contains just
the files + their commits that you specified above (``src/extractcode`` and 
``tests/extractcode``). However, the git history will contain many empty merge commits
from everything that was outside of the subset filtered above. To remove these empty merge
commits, run the following:
::

    git filter-branch -f --prune-empty --parent-filter \
    'sed "s/-p //g" | xargs -r git show-branch --independent | sed "s/\</-p /g"'

At this point, your repo will now contain the specified files + directories you pulled
out from and their git history. The final step is set the git remote for your "newly"
created repository and push the changes there:
::

    git remote set-url origin new-remote-url-here
    git push -u origin branch-name

To prevent pushing to the source git repository, it may be a good idea to remove the old
remote url:
::

    git remote rm old-remote-name
