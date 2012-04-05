.. raw:: html

   <span class="mytitle"> Intro to </span>
   <img width="750" style="padding: 10px;" src="images/git.gif"/>
   <span class="mytitle">and</span>
   <img width="300" style="padding: 10px;" src="images/github_logo.png"/>

   <span class="authors">
    Martin Luessi
   </span>

Martinos Center Why N How, April 5, 2012

----

..imagine the following scenario:

- You write some code (or a analysis script, LatTex code, etc.)
- You want to share it with others and collaborate on it

How do you do it?

- Send around e-mails?
- Put the file on a shared drive / dropbox?

Potential problems:

- It is difficult to track changes (someone needs to coordinate)
- What if two collaborators make incompatible changes?

----

Enter Version Control Systems..
-------------------------------

- Widely used for software development
- Allow multiple people to work on the same files
- Keep track of file changes
- Provide a structured way of integrating changes (branching and merging)
- Work with any type of text file (not binary files like .doc etc.)

----

What is git?
------------

- git is a **free** and **open source, distributed** version control system
- Originally created by Linus Torvalds in 2005 (Creator of Linux Kernel)
- It is designed to be fast

Unique features:

- Every **git clone** is a full **repository** with complete history etc.
- It does not have to rely on a centralized server (not like CVS or SVN)
- **Branching** and **merging** are easy to do and fast

----


Getting a git Repository
------------------------

Either **create** a new repository:

.. sourcecode:: bash

    [$] cd ~/mycode
    [$] git init
    Initialized empty Git repository in /home/martin/mycode/.git/

Or **clone** a repository:

.. sourcecode:: bash

   [$] git clone git://github.com/schacon/simplegit.git
   Initialized empty Git repository in /home/martin/simplegit/.git/
   remote: Counting objects: 100, done.
   remote: Compressing objects: 100% (86/86), done.
   remote: Total 100 (delta 35), reused 0 (delta 0)
   Receiving objects: 100% (100/100), 9.51 KiB, done.
   Resolving deltas: 100% (35/35), done.
   [$] ls -al simplegit
   total 24
   drwxr-xr-x 4 martin martin 4096 Apr  4 14:41 .
   drwxr-xr-x 9 martin martin 4096 Apr  4 14:41 ..
   drwxr-xr-x 7 martin martin 4096 Apr  4 14:41 .git
   -rw-r--r-- 1 martin martin  125 Apr  4 14:41 README
   -rw-r--r-- 1 martin martin  592 Apr  4 14:41 Rakefile
   drwxr-xr-x 2 martin martin 4096 Apr  4 14:41 lib


----

First Steps: Adding a File
--------------------------

see the **status**

.. sourcecode:: bash

   [$] cd ~/mycode
   [$] git status
   # On branch master
   #
   # Initial commit
   #
   nothing to commit (create/copy files and use "git add" to track)

- We are on the **master branch**
- The repository is empty

let's **add** a file

.. sourcecode:: bash

   [$] echo "hello git" >> test.txt
   [$] git add test.txt

----

First Steps: Adding a File Cont.
--------------------------------

see the **status** again

.. sourcecode:: bash

   [$] git status
   # On branch master
   #
   # Initial commit
   #
   # Changes to be committed:
   #   (use "git rm --cached <file>..." to unstage)
   #
   #       new file:   test.txt


**commit** all changes

.. sourcecode:: bash

   [$] git commit -a -m "my first file"
   [master (root-commit) cb2ff46] my first file
    Committer: martin <martin@think.(none)>
    1 files changed, 1 insertions(+), 0 deletions(-)
    create mode 100644 test.tx

see the **log**

.. sourcecode:: bash

   [$] git log
   commit cb2ff4663bdc3bf3d38a0ad534dd770656c45f0d
   Author: martin <martin@think.(none)>
   Date:   Wed Apr 4 15:10:42 2012 -0400
   my first file

----

Making More Changes
-------------------

Make modifications to the file

.. sourcecode:: bash

   [$] echo "new content" >> test.txt

See the **difference**

.. sourcecode:: bash

   [$] git diff
   diff --git a/test.txt b/test.txt
   index 8d0e412..ab04ca9 100644
   --- a/test.txt
   +++ b/test.txt
   @@ -1 +1,2 @@
    hello git
    +new content

And again **commit** the changes

.. sourcecode:: bash

   [$] git commit -a -m "more changes"
   [master cb7fe4f] more changes
   Committer: martin <martin@think.(none)>
   1 files changed, 1 insertions(+), 0 deletions(-)

----

Summary so far
--------------

- Use **git init** and **git clone** to create or clone a git repository, resp.
- Use **git status** to see the status
- Use **git add** to add a file/directory to version control
- Use **git diff** to see the changes you made
- Use **git commit** to commit your changes
- Use **git log** to see the log

----

Branching.. let the fun begin
-----------------------------------------

- So far we have been working on the **master branch**
- You usually want to make changes in a separate branch

Let's see what branches are available

.. sourcecode:: bash

   [$] git branch
   * master

so far we only have the **master branch**

Create a new branch

.. sourcecode:: bash

   [$] git branch my_branch


Switch to the new branch

.. sourcecode:: bash

   [$] git checkout my_branch

Change the file again and commit the changes

.. sourcecode:: bash

   [$] echo "even more content" >> test.txt
   [$] git commit -a -m "changes in branch"
   [my_branch 6354500] changes in branch
   Committer: martin <martin@think.(none)>
   1 files changed, 1 insertions(+), 0 deletions(-)


----

Branching Cont.
---------------

Let's switch back to the **master branch**

.. sourcecode:: bash

   [$] git checkout master


and look at the file

.. sourcecode:: bash

   [$] cat test.txt
   hello git
   new content

here the file is still the same. The changes we made are in ``my_branch``

We can checkout ``my_branch`` again and make more changes.

----

Merging Branches
----------------

Finally, we can **merge** the changes into the master branch

.. sourcecode:: bash

   [$] git checkout master
   [$] git branch
   * master
     my_branch
   [$] git merge my_branch
   Updating cb7fe4f..6354500
   Fast-forward
    test.txt |    1 +
     1 files changed, 1 insertions(+), 0 deletions(-)

Now, the master branch has the changes we made in ``my_branch``

.. sourcecode:: bash

   [$] cat test.txt
   hello git
   new content
   even more content

----

..this is all very nice, but
----------------------------

- How do you share a git repo amongst multiple people?
- You could put it on an shared drive / dropbox etc.

still:

- Managing permissions can be difficult
- It is difficult to keep track of who changes what
- You still need e-mail, IRC, etc. to coordinate and discuss changes

----

github to the Rescue
--------------------

- github is a company that specialized in git hosting
- It combines git with social networking
- Free for open source projects
- 1.3 million users, 2 million git repos (as of 2/2012)


.. image:: images/github_logo.png
   :scale: 50%


-----

Getting Started with github
----------------------------

- Create an account on `<https://www.github.com>`_
- Set up SSH keys see `<http://help.github.com/set-up-git-redirect>`_

- Set your name and e-mail address

- Either **create** a new git repository

.. image:: images/new_repo.png
   :scale: 150%

- Or find a project you want to contribute to and **fork** the repo

.. image:: images/fork.png
   :scale: 150%

-----

Getting Started with github Cont.
----------------------------------

- Clone the repository

.. sourcecode:: bash

  [$] git clone git@github.com:mluessi/gitexample.git

- Set your name and e-mail address

.. sourcecode:: bash

   [$] cd gitexample
   [$] git config user.name "Firstname Lastname"
   [$] git config user.email myemail@mail.com

**Important**: Use the same e-mail and name you use on github

- Start changing things, as we did before
- Remember: don't make changes in the master branch

- To keep your local repo up to date, **pull** changes from github

.. sourcecode:: bash

   [$] git pull

----

Workflow for Adding a Feature
-----------------------------

- Fork the repo on github and clone it to your machine (prev. slides)
- Create a new branch and check it out

.. sourcecode:: bash

   [$] git branch alg_optimization
   [$] git checkout alg_optimization


.. raw:: html

   <span class="smalltxt">

Tip: You can do the same using ``git checkout -b alg_optimization``

.. raw:: html

    </span>

- Make your changes, commit them to the branch
- So far, all your changes are local, github does not know about them
- You need to **push** the branch to github

.. sourcecode:: bash

   [$] git push origin alg_optimization

Note: ``origin`` is an alias for a remote repo, you can configure them using ``git remote``

-----

PR: Get Your Changes Included
---------------------------------------

- Go to your repo on github
- Switch to your feature branch

.. image:: images/switch_branch.png
   :scale: 150%

- Make a **Pull Request (PR)**

.. image:: images/pull_request.png
   :scale: 150%

This will:

- Send an e-mail notification to all authors
- The PR can be discussed on github
- You can keep pushing changes to your branch until everyone is happy
- Finally, the owners of the original repo can merge your changes

.. image:: images/merge_pr.png
   :scale: 100%


----

Live Demo
---------


bla



----


Find out More
-------------

- On git `<http://git-scm.com/documentation>`_
- About github the company `WIRED: Lord of the Files: How GitHub Tamed Free Software <http://www.wired.com/wiredenterprise/2012/02/github/all/1>`_
- Details on `how to contribute to a project <http://martinos.org/mne/gitwash/git_development.html>`_
- Trick: `show current branch in BASH prompt <https://github.com/kura/git-current-branch-bashrc>`_


----

Finally..
---------

.. raw:: html

   <div class="centerslide">
   Questions?
   </div>




















































