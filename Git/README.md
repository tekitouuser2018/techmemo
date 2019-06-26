# Git
-----

- Clone remote repository
    - git clone -b [remote branch name]
    - git@github.com:USER_NAME/[repository address].git  

- Create local branch + move
    - git checkout -b [local branch name] origin/[remote branch name]  

- Chech remote
    - git remote -v
    - git branch --remote  

- Staging
    - git add [file name]
    - git add .  

- Commit
    - git commit -m "Message"

- Pull
    - git pull origin [branch name]  

- Push
    - git push origin [local branch name]
    - git push origin [local branch name]:[remote branch name]
    - git push origin [local branch name] //push to origin that same name with local
    - git push origin HEAD   

- Merge
    - git merge [designate branch] //HEAD branch merge designate branch  

- Check the changes of specific commit
    - git show [commit id]
    - git diff [branchA] [branchB]
    - [GitHub Address] + [LEFT:- RIGHT:+] // ghttps://github.com/tekitou/project/compare/{feature/id/1}...{feature/id/2}  

- Reset the change of file
    - git checkout -- [file name]  

- Reset the all changes of not committed
    - git checkout -f
    - git reset --HARD
    - git checkout .

- Back to one step before
    - git reset --hard HEAD  

- VSCode -compare diff
    - ⌘⬆️P(Command Parret)→>compare:~  

- Change the local branch name
    - git branch -m(old branch name(no need if current branch)) [new branch name]  

- Solve the conflict
    - git pull --rebase origin [local branch name]  

- Erase local branch
    - git push --delete [branch name] //erace branch of merged to HEAD
    - git push -D [branch name] //erace nether merged or not  

- Cancel staging
    - git reset [file name] //cancell staging. edit contents will remain
    - git checkout HEA -- [file name] //staging file back to last committed state. edit contents will NOT remain
    - git reset --hard HEAD //cancell the changes that either edit and staging. Back to last committed state  

- Roll back commit version
    - git reset --hard [commit id]  

- Roll back specific file commit version
    - git checkout [commit id] [file path]  

- Cancel the merge
    - git reset --hard HEAD^ //back local branch to one step before
    - git push --force origin [branch name] //force change to remote repository