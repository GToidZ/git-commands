## Commands for Remotes

1. List all your remote repositories and show their URLs:
   ```
   git remote -v
   ```

2. View details about a remote repo named `origin`, including all the remote branches and local tracking branches for `origin`:
   ```
   git remote show origin
   ```


3. (Pushing a new branch) You commit some files to the `dev-foo` branch and try to "push" them to Github, but it fails as shown here:
   ```
   cmd>  git checkout dev-foo
   cmd>  git push
   fatal:  The current branch dev-foo has no upstream branch. 
   ```
   Explain this error.
   > The error is that you tried to push changes from the local `dev-foo` branch to remote `dev-foo` branch but since Git hasn't been configured with an upstream branch (matching remote branch), it fails to push.

4. The command to push `dev-foo` to `origin` as a **new remote branch** on `origin` is:
   ```sh
   # Or use -u for shorthand alias
   git push --set-upstream origin dev-foo
   ```

5. (Create a local tracking branch for a remote branch) The remote repository (`origin`) has a branch named `e2e-test` that you don't have in your local repository.   
   The command to create a new local branch as a copy of the remote `e2e-test` branch that **tracks** the remote branch is:
   ```sh
   # 1. Pull and apply (Fetch & Merge)
   git pull
   # or 'git pull origin e2e-test' to pull specific branch
   git checkout e2e-test

   # 2. Fetch-only, no merging (Still retrieves all branches anyways)
   git fetch origin
   git checkout e2e-test

   # 3. Fetch only that branch (Using FETCH_HEAD)
   # Seemed like a way to minimize trackers, ref: https://git-scm.com/docs/git-pull/#_examples
   git fetch origin e2e-test
   git branch e2e-test FETCH_HEAD
   git checkout e2e-test
   ```

6. Consider this situation:
   - you have a local repository with a remote repo on Github
   - Your local repo is up-to-date with the remote repo on Github!
   - You edit `README.md` on Github using Github's web interface and save the changes.
   - On your local machine, you edit README.md, commit the changes locally
   
   What happens when you `push` your changes?    
   Explain why and how to fix it.
   > **The push will fail.** Git will output an error message, stating that changes are not up-to-date with the remote.
   
   > ***Why?*** When you try to push local changes to remote repository in this matter, the push has a parent commit of commit before you edited the file on GitHub.
   >
   > This is the first reason why it will fail.
   >
   > To fix the issue, use `git fetch` to update the index of local repo to be equivalent to remote repo.
   >
   > Then, use `git merge --no-commit` to try automatic merge first, if you didn't an error, skip the next step.
   >
   > To fix another issue that automatic merge cannot be made, you will need to modify the conflicting files yourself until all the conflicts are resolved.
   >
   > Don't forget to `git add` any resolved files, commit and push them again.

7. The command to change the URL of the remote "origin" to a new URL, such as `https://hostname/newuser/new-repo-name`, is:
   ```
   git remote set-url origin https://hostname/newuser/new-repo-name
   ```
   This situation occurs when:
   - you change the name of a repo on Github
   - you transfer ownership of a Github repo to someone else
   - you move from Github to another hosting site, like Bitbucket
   - you want to switch from the https to the ssh protocol (the remote URL is different even though location is the same)    

8. To create a *second* remote repository for your local repo, the command to add a remote named "bitbucket" with the URL "https://bitbucket.org/your-username/git-commands" is:
   ```sh
   # TODO: Make BitBucket git-commands repo
   git remote add bitbucket https://bitbucket.org/your-username/git-commands
   ```
   - Note: you must **create** an empty repo on Bitbucket. This command just adds it as a remote, it won't create the remote repo.

9. After adding the remote named `bitbucket`, the command to push your master branch to `bitbucket` is:
   ```
   git push bitbucket master
   ```

