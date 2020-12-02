# Consolidated Notes for using Git


## Best Practices 
### Working on a branch
Good practice to update your branch to the latest master to minimize occurrences of conflicts. We can do this in 2 ways

1. It is a good practice to occassionally run the following
     ```
    git fetch
    ```
    `fetch` fetches the metadata of your remote repository to your local repository. Metadata would include commits that were made and pushed to the remote. Hence, `fetch` helps to keep your local repo aware of the developments in the remote. 
    
2.  After running `fetch`, you may run the following to identify specific differences in your local files to the remote files
     ```
    git diff <current branch name> origin/master
    ```
    Note that the syntax `origin` is absolutely critical as it refers to the master from the remote repository, **NOT YOUR LOCAL master**. `origin` is an default alias for the remote url and can always be changed. Typing in `git remote -v` makes this clear that origin is akin to the name of an object you assign to in R. Clearly, if you wish to perform a similar execution with respect to your local repository, simply drop the syntax `origin`.  This command helps you to check if there are any major changes in the origin's master that might have conflicts with your development. If there are you can rectify it before merging or rebasing. If there are no changes and your folder is up to date, you may simply proceed with the development.
    
3.  Assuming there are changes, then you would want to do the following
    ```
    git merge origin/master
    ```
    This helps to merge any commits made in the master branch to the current working branch. Frequent merging (or at least running the command) is good practice because if commits in master is ahead of the branch you work on, conflicts, if any, can be easily addressed before the master and working branch deviates too much such that it becomes difficult to resolve.
    
4. If you are confident that there would be no serious conflicts or you are just plain old lazy, as any normal intelligent being would be, you might choose to do steps 1 and 3 simultaneously by running
    ```
    git pull origin master
    ```
    If a conflict occurs, the `pull` will be incomplete and conflicts will have to be rectified before you can complete this task.
    
5. Alternatively, instead of merging, we might want to run a `rebase`
    ```
    git rebase origin master
    ```
    In this example, we defined branch is the master branch. Merge migrates the history of the changes that were made independently on master (and not updated on current branch) to the current working branch but the commits on botht he working branch and master are recorded as they happened. In other words, commits made on master are seen as commits on master while commits on the current branch are preserved as being made int he current branch. On the other hand, when rebasing, the commits made on master are then seen as commits on the current branch and previous commits on the current branch are then **pushed to the head of the branch** (another way to phrase it is that it is pushed ahead of the master commits).
   
    Based on the description we can see that we might not want to execute rebase on the working branch from the master branch as it rewrites the history of the commits. However, this might be favourable if you wish the key developments and updates to be clear and follow a chronological order. As a recommendation to the team, for the sake of simplicity, we should stick to merging for now until we are more comfortable and clear between the advantages and disadvantages of merging and rebasing.
    
    A thought of where we might want to use rebase would be in more complex tasks. Take for example a large scale data preparation execercise. In this case, we might have a main data preparation branch where it **acts** like the master branch but serves the sole purpose of developing data preparation codes. Here, we might have several sub branches branching out from this branch and in this case, it might make sense to rebase the main data preparation branch from updates in the sub branches to minimize challenges trying to track complicated histories.
    
  [Click here for an excellent source of reference.](https://stackoverflow.com/questions/21756614/difference-between-git-merge-origin-master-and-git-pull) You will find that rebasing is recommended but we are warned that we should never rebase commits that have been pushed by others as you would totally screw up their work. Examples of disasters that might happen can be found [here (warning - technical)](https://git-scm.com/book/en/v2/Git-Branching-Rebasing) under the header _The Perils of Rebasing_. Basically, we might screw up the history. Because of this, lets stick to merges for now until we're really proficient.
    

## Notes for WHAT IF SCENARIOS
1.	If you’ve done and committed work in the wrong branch and want it to be updated to the branch that you intend to work on, do the following
    Key in the following into git bash: `git reset --soft HEAD^`
    Followed by switching to the originally intended branch before commiting the updates into the current branch

    Notes (rough and non technical explaination. Don’t take at face value ): 
    -	soft means to undo the commit without undoing the changes 
    -	HEAD^ indicates the most recent commit
