# Quick Guide to Git

By Pawitchaya C.

## Using Git

[Basics](#basics)    
[Adding and Changing Things](#adding-and-changing-things)    
[Undo Changes and Recover Files](#undo-changes-and-recover-files)    
[Viewing Commits](#viewing-commits)    
[Commands for Remotes](remote-commands.md)   
[Useful Commands](#useful-commands)     
[Resources](#resources)

#### Note on Paths

In this file, directory paths are written with a forward slash as on MacOS, Linux, and the Windows-Bash shell: `/dir1/dir2/somefile`.    

## Basics

1. When using Git locally, what are these?  Define each one in a sentence
   * Staging area - A part of the Git work lifecycle where files with changes are enqeued by user before making a snapshot (commit).
   * Working copy - A Git repository that is on a local computer which can contain changes to the repository but is not yet committed to the repository.
   * master - The default/common name for a branch in a Git repository; most of the repositories will have an initial branch with this name.
   * HEAD - A reference to a commit object, which usually points to the tip of the current active branch.

2. When you install git on a new machine (or in a new user account) you should perform these 2 git commands to tell git your name and email.  These values are used in commits that you make:
   
   ```
   # Git configuration commands for a new account
   git config --global user.name Name
   git config --global user.email name@example.com
   ```

3. There are 2 ways to create a local Git repository.  What are they?
   * **Create a new repository from scratch:**
   
     Use a **terminal** to navigate to an empty folder or any folder you wanted to make a Git repository. (usually `cd`)
     
     Then use the following command inside the terminal,
     
     ```
     git init
     ```
     
     You will get a new Git repository and a working copy as that directory you've executed the command inside. The repository will have no remotes to push changes to yet.
   
   * **Clone a repository from a Git-supported repository hosting services:**
   
     Use a terminal to navigate to a destination folder you wanted to clone the repository to.
     
     Then use the following command inside the terminal,
     
     ```
     git clone <repository-url> [directory-name]
     ```
     
     You will have a working copy as a new directory in the destination folder with the same name as the repository unless you set the `directory-name` option. The repository will already have remote to the repository online.

4. When you create a git repository by entering `git init`, Git will create a "hidden" directory for the local repository.  Where is the directory for this local repository (relative to the directory where you typed "git init")?

   The hidden directory `.git` is in the location where `git init` is executed. For example,
   
   ```sh
   ~ > cd my-repo
   my-repo > ls -a
   .git
   ```

## Adding and Changing Things

Suppose your working copy of a repository contains these files and directories:
```
README.md
out/
    a.exe
src/a.py
    b.py
    c.py
test/
    test_a.py
    ...
```     

1. Add README.md and *everything* in the `src` directory to the git staging area.
   ```
   git add README.md src/
   ```

2. Add `test/test_a.py` to the staging area (but not any other files).
   ```
   git add test/test_a.py
   ```

3. List the files in the staging area.
   ```
   git status
   ```

4. Remove `README.md` from the staging area. (Useful if you accidentally add something you don't want to commit.)
   ```
   git rm --cached README.md
   ```
   
5. Commit everything in the staging area to the repository.
   ```
   git commit -m "added files to repository"
   ```

6. Describe 2 steps to configure the repository so git will ignore all files in the `out/` directory:
   - Create a new text file named `.gitignore` in the repository folder.
   - In `.gitignore`, save it with the following text,
     ```
     out/
     ```

7. Command to move all the .py files from `src` to the top-level directory of this repository, so they are also moved in the Git repo.
   ```
   git mv src/*.py ./
   ```

8. Commit this change with the message "moved src directory":
   ```
   git commit -m "moved src directory"
   ```

9. Command to add **all changed files** (but not untracked files) to the staging area using a single command.
   ```
   git add --update
   ```

10. **Delete** the file `c.py` from your working copy **and** the repository:
    ```
    git rm c.py
    ```

## Undo Changes and Recover Files

1. Display the differences between your *working copy* of `a.py` and the `a.py` in the *local repository* (HEAD revision):
   ```
   git diff HEAD a.py
   ```

2. Display the differences between your *working copy* of `a.py` and the version in the *staging area*. (But, if a.py is not in the staging area this will compare working copy to HEAD revision):
   ```
   git diff a.py
   ```

3. **View changes to be committed:** Display the differences between files in the staging area and the versions in the repository. (You can also specify a file name to compare just one file.)
   ```
   git diff --staged
   ```

4. **Undo "git add":** If `main.py` has been added to the staging area (`git add main.py`), remove it from the staging area:
   ```
   git restore --staged main.py
   ```

5. **Recover a file:** Command to replace your working copy of `a.py` with the most recent (HEAD) version in the repository.  This also works if you have deleted your working copy of this file.
   ```
   git restore a.py
   ```

6. **Undo a commit:** Suppose you want to discard some commit(s) and move both HEAD and "master" to an earlier revision (an earlier commit)  Suppose the git commit graph looks like this (`aaaa`, etc, are the commit ids)
   ```
   aaaa ---> bbbb ---> cccc ---> dddd [HEAD -> master]
   ``` 
   The command to reset HEAD and master to the commit id `bbbb`:
   ```sh
   # While on branch master
   git reset bbbb
   ```

7. **Checkout old code:** Using the above example, the command to replace your working copy with the files from commit with id `aaaa`:
   ```sh
   # Using --hard option will also replace files within working copy
   git reset --hard aaaa
   ```
    Note:
    - Git won't let you do this if you have uncommitted changes to any "tracked" files.
    - Untracked files are ignored, so after doing this command they will still be in your working copy.

## Viewing Commits

1. Show the history of commits, using one line per commit:
   ```
   git log --oneline
   ```
   Some versions of git have an *alias* "log1" for this (`git log1`).

2. Show the history (as above) including *all* branches in the repository and include a graph connecting the commits:
   ```
   git log --all --graph
   ```

3. List all the files in the current branch of the repository:
   ```
   git ls-files
   ```
   example output:
   ```
   .gitignore
   README.md
   a.py
   b.py
   test/test_a.py
   test/test_b.py
   ```

## Branch and Merge

1. Create a new branch named `dev-foo`:
   ```
   git branch dev-foo
   ```
 
2. Display the name of your current branch:
   ```
   git branch
   ```

3. List the names of **all** branches, including remote branches:
   ```
   git branch --all
   ```

4. Switch your working copy to the branch named `dev-foo`:
   ```
   git checkout dev-foo
   ```

5. **Merge:** To merge the work from `dev-foo` into the master branch, perform these steps:
   1. Checkout at the `master` branch.
      ```
      git checkout master
      ```
   2. Merge the `dev-foo` branch with `master` branch.
      ```
      git merge dev-foo
      ```

6. Describe under what conditions a merge may fail.
   
   Merging can fail whenever the two or more branches have modified the same part (lines) of a file. In Git, it is considered a conflict and must be resolved using `mergetool`.
   
## Useful Commands

**Rollback a commit**: I often had nightmares about committing wrongly and pushing them into the remote repository, so I wrote something explicit but works personally,
```sh
# Restting to HEAD^ means commit down in the working tree by 1.
# Using --soft means you'd not lose changes to local files.
# Second command will unstage the files.
git reset --soft HEAD^ && git reset HEAD .
```

---
## Resources

[Pro Git Online Book][ProGit] Chapters 2 & 3 contain the essentials. Downloadable PDF is also available.     
[Visual Git Reference](https://marklodato.github.io/visual-git-guide) one page with illustrations of git commands.

Try Git:

[Learn Git Interactive Tutorial][LearnGitInteractive] excellent visual tutorial.   
[Git Visualizer][VisualizeGit] execute Git commands in a web browser and see the results as a graph.

[Video Playlist][Git for Poets] by [The Coding Train][TheCodingTrain], has short episodes and suitable for those who are new to GitHub concepts.


[ProGit]: https://www.git-scm.com/book/en/v2 "Pro Git online book on Git-scm.com"
[ProGitPdf]: https://progit2.s3.amazonaws.com/en/2016-03-22-f3531/progit-en.1084.pdf "Pro Git v.2 PDF on AWS. Longer, book format."
[LearnGitInteractive]: https://learngitbranching.js.org "Interactive graphical git tutorial"
[VisualizeGit]: http://git-school.github.io/visualizing-git/ "Online tools draws a graph of commits in a repo as you type"
[markdown-cheatsheet]: https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet
[github-markdown]: https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax
[vscode-markdown-preview-enhanced]: https://marketplace.visualstudio.com/items?itemName=shd101wyy.markdown-preview-enhanced
[TheCodingTrain]: https://www.youtube.com/c/TheCodingTrain
[Git for Poets]: https://www.youtube.com/playlist?list=PLRqwX-V7Uu6ZF9C0YMKuns9sLDzK6zoiV
