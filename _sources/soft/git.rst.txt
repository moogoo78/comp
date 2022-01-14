********************
GIT
********************
:date: 2013-03-23
:category: computer
:tags: docs

How to
============

sync remote repo
------------------

.. code-block:: bash

   $ git fetch <remote name> # fetch specific remote branches
   $ git fetch --all # fetch all remote branches
   # same as
   $ git remote update

   git pull # has upstream
   # same as
   $ git fetch <remote name> && git merge <remote name> <branch name>
   $ git pull <remote name> <branch_name> # no upstream
   $ git pull --rebase


減少 commit(merge commit )
----------------------------------

善用 git revert, git-bisect, git-rebase -i

`5 Reasons for Keeping Your Git Commits as Small as You Can | Crealytics GmbH <https://crealytics.com/blog/2010/07/09/5-reasons-keeping-git-commits-small/>`__

::

   # a -> b1 -> b2 要變成 a -> b
   git-rebase -i [a-hash]

   # pick b1
   # pick b2 # 改成 squash b2
   # 修改 commit message (可以 commit b2 的 message)
   # 存檔離開就好了



git clone某一個branch
----------------------------
以github的Flask當例子，要抓Flask的website branch

先git clone一份Flask，看所有的branch::

  git branch -a

可看到除了master外有一個remotes/origin/website，然後做一個叫website的branch::

  git checkout -b website remotes/origin/website

就會抓回到website這個branch，或是要直接拉下來看::

  git checkout remotes/origin/experimental


git format-patch  把修改紀錄依照 commit 分別作出 diff 來
------------------------------------------------------------

  git format-patch [HASH] # 把從這個 [HASH] 之後 (不包括[HASH]) 的 commit 都分成一個個檔案的 patch



commit 錯 branch
-----------------------

.. code-block:: bash

    git reset --hard HEAD~1
    # working dir 就會有 還沒 commit 前的狀態
    git checkout b newbranch
    # 重新 git add/commit 就好了

commit empty folder
---------------------

add a .gitignore in folder as follows

.. code-block::

   # Ignore everything in this directory
   *
   # except this file
   !.gitignore



Reference
============

git clone / add
------------------
git clone::

    git clone file:////home/somebody/repo/foo.git
    git clone ssh://host/~/repo/foo.git
	git clone --bare /path/to/repo # 把原本的repo轉成bare出來


clone all branches::

  $ git clone --mirror git://example.com/repo repo.git # clone all branches
  $ mkdir new_proj
  $ cd new_proj
  $ mv repo.git new_proj/.git
  # remove bare=true in .git/config



.. note:: git pull origin master相當於git fetch和git merge origin/master

git add::

    git add .
    git add -a
    git add foo
    git add -u # 只加修改過的檔案, 新增的檔案不加入.
    git add -i # 進入互動模式


git rm/mv::

    git rm foo
    git mv foo bar

    git rm --cached # 從版本控管裡那掉，保留原檔


git reset / revert
---------------------


reset: 移動HEAD, 更改SHA referenct(git記錄), 檔案還是目前最新的狀態
還沒commit

revert: rollback, 並且會有一個新的commit(不像reset是回到某個commit) 會保留 commit log

example::

  # 原本
  A -> B -> C ->

  # revert, 回到 B, 產生一個新 commit (一路往前)
  git revert C
  A -> B -> C -> B ->

  # reset, 清掉 commit log, 檔案保留目前狀態 (C變成 unstaged)
  git reset B
  A -> B ->

  # rebase, 同步 remote 跟 local
  # rebase -i 拿掉某些 commit 不想留下記錄 (要 pick...)


* `Pro Git - Reset Demystified <http://progit.org/2011/07/11/reset.html>`__
* `Reset, Checkout, and Revert | Atlassian Git Tutorial <https://www.atlassian.com/git/tutorials/resetting-checking-out-and-reverting>`__

git reset::

    git reset --soft HEAD~ # HEAD指回the parent of HEAD, Index和Working Directory都不動(因為soft嘛)
    # 改一改再commit一次, 就是git commit --amend在做的事情一樣
    (修改放到staging area?)
    git reset [--mixed] HEAD~ # update Staging Area, 沒有打參數預設就是這個 (把stage的檔案給unstage?)
    git reset --hard HEAD~ # 完全清除, reset the working tree to the last commit
    #會改到檔案要注意

Tips: 如果反悔可以用git reflog叫出來, 再reset回來

commit後要回來
git revert (還原commit記錄) # revert是新增一個commit來做還原

.. note:: push後就不能reset了, 如果已經push後就要用revert

git revert::

    git revert HEAD # 回上一次commit, 並產生一個新的commit log
    This will create a new commit which undoes the change in HEAD
    git revert HEAD^ # 回上上一次commit
    git revert [SHA] # 回到此commit **之前**\的狀態, 也是產生一個新的commit log

git log
-----------
git log::

    git log --stat
    git log --stat --summary # 查每個版本間的更動檔案和行數
    git log --pretty=format:'%h : %s' --date-order --graph # 依照時間排序
    git log --since="1 day ago" # 今天的
    git log --pretty=format:"%h was %an, %ar, message: %s" (--graph)
    git log --all # 秀出所有的 log (含 branch)
    git log -p # 將所有 log 和修改過得檔案內容列出
    git log -S'foo()' # log 裡面有 foo() 這字串的.
    git log --pretty=oneline # 秀 log 的方式
    git log --pretty=short # 秀 log 的方式
    git log --pretty=format:'%h was %an, %ar, message: %s'
    git log --pretty=format:'%h : %s' --graph # 會有簡單的文字圖形化, 分支等.
    git log --pretty=format:'%h : %s' --topo-order --graph # 依照主分支排序
    git log --pretty=format:'%h : %s' --date-order --graph # 依照時間排序

    git rev-list --count [HEAD/master...] # sum commits
    git shortlog -s # sum by author


awesome analytic git log, **gitstats**::

  $ apt-get install gitstats
  $ gitstats /path/to/repo output_dir #ex: gitstats . out

`GitStats - git history statistics generator <http://gitstats.sourceforge.net/>`__



git commit / checkout / stash
-------------------------------
git commit::

    git commit -m 'foo message'
    git commit --dry-run # 假commit
    git commit --no-verify # 不檢查每行程式碼最後是否有空白或tab
    git commit --amend -m 'edited message' # 改最後一次commit的的message
    # 通常出現錯誤"You have some suspicious patch lines"時用

git checkout::

    git checkout branch # 切換 branch
    git checkout filename # 從local repo抓回
    git checkout HEAD .  TODO
    git checkout xxxx . # 將所有檔案都 checkout 出來(xxxx commit 的版本, xxxx 是 commit 的編號前四碼), 注意, 若有修改的檔案都會被還原到上一版. TODO

git stash::

    git stash #  丟進暫存區
    git stash list # 列出所有暫存區的資料
    git stash pop # 取出最新的一筆, 並移除.
    git stash apply # 取出最新的一筆 stash 暫存資料. 但是 stash 資料不移除
    git stash clear # 把 stash 都清掉


git branch / merge
--------------------
git branch::

    git branch # 看目前的branch
    git branch abcd # 開一個名為abcd的新branch
    git branch -d abcd # 刪除abcd
    git checkout abcd, 移到abcd
    git branch abcd # 開新branch, 但不移駕
    git co -b abcd 開新branch, 而且移駕到新開的branch
    git branch -m old new # rename branch
    git branch -a # --all, list both remote-tracking branches and local branches
    git brnach -r # list or delete (-d) the remote-tracking branches

git merge::

    git mergetool kdiff3
    git merge --abort, 不要merge了

    git fetch origin
    git rebase -i origin/master


git分master跟branchA, 在master下 **$ git merge branchA** 會把branchA更改過的merge到master裡, 反之, 在branchA下 **$ git merge master** 會把master更新的家到branchA, branchA改過的不會影響master

git tag
-------
加tag::

  git tag -a v0.2 -m "this is version 0.2"
  git tag -a v0.1.1 #abc123 為某版本加tag

看此tag的log::
  git show v0.1.2

刪除::

  git tag -d v0.1.2


other
-----
git reflog ::

   把之前的記錄都叫出來, 包括被git reset掉的, 如果時間久的話會被git洗掉
   然後再$ git reset --hard HASH 就回去了

`version control - Undoing a git reset --hard HEAD~1 - Stack Overflow <http://stackoverflow.com/questions/5473/undoing-a-git-reset-hard-head1>`__



undo
----

* 還沒commit (local)

  * ``git reset --hard HEAD``
  * ``git checkout HEAD hello.rb``

* push 後發現錯誤 (public)

  * ``git revert HEAD`` # 回上個commit, 並建立一新commit
  * ``git revert HEAD^`` # 回上上個commit, 並建立一新commit

* 改 comment message

  ``git commit --amend``

* [branch] 在 master 改完, 要把更新改到 branch

  ``git branch feature``, ``git reset --hard origin/master``, and ``git checkout feature`` # 不要用 ``git checkout -b`` (因爲還沒要切到 branch, 只是先開 branch)

* 大量 undo/redo

  改了一堆後, 發現另一個方法比較好, 有些 commit 要, 有些不要

  ``git rebase -i <earlier SHA>`` # -i: interactive, squash, fixup, pick


* `Git Book - Undoing in Git - Reset, Checkout and Revert <http://book.git-scm.com/4_undoing_in_git_-_reset,_checkout_and_revert.html>`__
* `How to undo (almost) anything with Git <https://github.com/blog/2019-how-to-undo-almost-anything-with-git>`__ 好用!

找回刪掉的檔案

.. code-block:: shell

   git log -- path/to/file
   git checkout {second to last commit} -- path/to/file

* `Find and restore a deleted file in a Git repository - Stack Overflow <https://stackoverflow.com/questions/953481/find-and-restore-a-deleted-file-in-a-git-repository>`__

repo
-------

換 origin::

  git remote -v ;看 remote name
  git remote set-url origin https://your.repo.git


Quick Start
============

**Tree Roles**

:HEAD: 最後一次的commit (Repo)
:Index: 目前要commit的 (Staging)
:Working Dir: 目前修改工作區

set editor::

  git config --global core.editor "vim"
  export GIT_EDITOR=vim
  # or
  export VISUAL=vim
  export EDITOR="$VISUAL"
  # VISUAL 跟 EDITOR 都要設



init
-----
Server (Repository)::

    mkdir example.git
    cd example.git
    git --bare init # init加--bare, 表示只會在當下目錄放git的東西，不會有原始檔案, 如果要原始檔案再git clone出來就有了

Client::

    mkdir example
    cd example
    git init
    touch README
    git add README
    git commit -m 'first commit'
    git remote add origin ssh://USERNAME@REMOTE_SERVER/~/example.git
    git push origin master (第一次之後只要打git push就可以了)


Remote branch::

  local(foo)> git push -u origin foo
  remote> git checkout -b foo_test origin/foo # 可以不同branch name
  # 之後
  remote(foo_test)> git pull

  # pull remote branch
  $ git branch -f remote_branch_name origin/remote_branch_name
  $ git checkout remote_branch_name
  $ git push origin --delete <remote_branch_name>

  # push to remote branch
  $ git push origin local_branch_name:remote_branch_name



Links
============

* `寫給大家的 Git 教學 <http://www.slideshare.net/littlebtc/git-5528339>`__
* `A successful Git branching model » nvie.com <http://nvie.com/posts/a-successful-git-branching-model/>`__ git開發web方法
* `github/gitignore at master - GitHub <https://github.com/github/gitignore>`__ gitignore大全
* `git ready » learn git one commit at a time <http://gitready.com/>`__ git教學文(因材施教)
* `Why Git is Better Than X <http://whygitisbetterthanx.com/>`__
* `Git Magic - Preface <http://www-cs-students.stanford.edu/~blynn/gitmagic/>`__
* `Git 教育訓練課程投影片 (2012) | ihower { blogging } <http://ihower.tw/blog/archives/6696/?utm_source=feedburner&utm_medium=feed&utm_campaign=Feed:+ihower+({|ihower.tw|+blog+})&utm_content=Google+Reader>`__
* guides (快速) `guides/protocol/git at master · thoughtbot/guides <https://github.com/thoughtbot/guides/tree/master/protocol/git>`__
* `🌳🚀 CS Visualized: Useful Git Commands - DEV Community 👩‍💻👨‍💻 <https://dev.to/lydiahallie/cs-visualized-useful-git-commands-37p1>`__


setting
==========

Git 2.0 的push.default把預設的設定從 `matching` 改稱 `simple`, matching: 預設把目前全部branch push上去, simple只會push目前所在的branch::

  git config --global push.default [matching|simple...]

`git-config <http://git-scm.com/docs/git-config.html>`__


Tools
================

- `casperdcl/git-fame: Pretty-print `git` repository collaborators sorted by contributions. <https://github.com/casperdcl/git-fame>`__
  git-fame
- `arzzen/git-quick-stats: ▁▅▆▃▅ Git quick statistics is a simple and efficient way to access various statistics in git repository. <https://github.com/arzzen/git-quick-stats>`__
  git-quick-stats
