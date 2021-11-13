# Devops
## Instruction 1:
1. Programmer A creates two files (PRG1,PRG2) in working
directory and makes changes different changes in those files
and separately tracks in local git repository
- Ans.
     1. mkdir Documents/Team3/Devops
     2. cd Documents/Team3/Devops
     3. git init
     4. touch PRG1.txt // save the file and close
     5. touch PRG2.txt // save the file and close
     6. git add PRG1.txt PRG2.txt
2. Checks the status and take s the log of different activities. At this
point, he has two files under source control
- Ans. 
     1. git status
     2. git log
     3. git commit -m "First commit in main branch"
     
3. He also keeps the tracking copies on remote Git Hub repository
DevOps using Git commands
   - Ans. 
       1. git remote add origin https://username:githubtoken@github.com/username/repositoryname.git
       2. git push origin main
4. <Takes the snapshot of the Screen- SCREEN SHOT 1 >
  ![s_1](https://github.com/pskarthick15/Devops-1/blob/main/Screenshot%20from%202021-08-26%2016-41-25.png)
  
## Instruction 2:
  
1. After some time, the same programmer starts working on a
different PROFIT-Loss module on a new Branch1 and modifies
the second file PRG2 two times and keeps under source
control.
  - Ans. 
    1. git checkout -b Branch1
    2. git add PRG2.txt // After adding some content to the file PRG2.txt
    3. git commit -m "Profit-Loss module1"
    4. git add PRG2.txt // After adding some content to the file PRG2.txt
    5. git commit -m "Profit-Loss module2"
  
2. While Programmer A is managing his version of files in Branch1,
Programmer B working in evening shift adds different module
called PROFIT-Correction on Branch2 and edits the same file
PRG2. He also maintains different versions of the two changes in
git separately
  - Ans.
    1. git checkout -d Branch2
    2. git add PRG2.txt // After adding some content to the file PRG2.txt
    3. git commit -m "Profit-correction module1"
    4. git add PRG2.txt // After adding some content to the file PRG2.txt
    5. git commit -m "Profit-correction module2"
3. After two days of module development code changes, both
Programmer A and Programmer B merges their files and pushes
local main repository to remote git hub repository DevOps
  - Ans.
      1. git checkout main
      2. git merge Branch1 Branch2
      3. git push origin main
4. They delete both Branch1 and Branch2
  - Ans.
    1. git branch -d Branch1
    2. git branch -d Branch2
5. He checks the status and maintains the history of the files using
Git commands
  - Ans.
    1. git status
    2. git log
6. <Take the snapshot of the git commands and git hub screen –
SCREEN SHOT -2>
  ![s_2.1](https://github.com/pskarthick15/Devops-1/blob/main/Screenshot%20from%202021-08-26%2017-02-27.png)
  ![s_2.2](https://github.com/pskarthick15/Devops-1/blob/main/Screenshot%20from%202021-08-26%2017-02-34.png?raw=true)
  
## Instructions - 3:
  - Identify merge conflict and solve
1. Next day, Programmer A clones the latest DevOps directory
from the Git Hub DevOps and starts working on it.
  - Ans. 
      1. git clone https://username:githubtoken@github.com/username/repositoryname.git
2. Programmer A creates Branch3 makes some changes on PRG1.
He does not want to put under tracking until the modification
is complete on the file.
  - Ans.
      1. git checkout -b Branch3
      2. git stash // do after mofifying some content
      
3. Now, He switches to main branch to edit the same file PRG1
for some modification.
  - Ans.
    1. git checkout main
    2. git stash list // to list out all the ids of stashed made
    3. git stash apply {id}// to apply the contents from stashed file using the id listed from above
    4. git add PRG1.txt
    5. git commit -m "Commit after the stash is over"
4. Merge conflict appears when the same copies of the file PRG1
is edited in two branches. Create Merge Conflict situation
  - Ans.
    1. git merge Branch3 // Now it throws a merge conflict error due to same lines of same file at 2 different branches.
    
5. Resolve the conflict and complete the Commit
  - Ans. 
      1. // Now we manually resolve the merge conflict by accepting changes from either of the branches or from both
      2. git add PRG1.txt
      3. git commit -m "Commit after the merge conflict is resolved"
6. <Take the snapshot (SCREEN SHOT 3) of the Git commands
executed for the above steps i.e before Merge Conflict and after
solving the Merge Conflict>
      ![s_3.1](https://github.com/pskarthick15/Devops-1/blob/main/Screenshot%20from%202021-08-26%2018-22-15.png)
      ![s_3.2](https://github.com/pskarthick15/Devops-1/blob/main/Screenshot%20from%202021-08-26%2018-22-17.png?raw=true)
      ![s_3.3](https://github.com/pskarthick15/Devops-1/blob/main/Screenshot%20from%202021-08-26%2018-24-05.png)
      ![s_3.4](https://github.com/pskarthick15/Devops-1/blob/main/Screenshot%20from%202021-08-26%2018-24-14.png?raw=true)
      ![s_3.5](https://github.com/pskarthick15/Devops-1/blob/main/Screenshot%20from%202021-08-26%2018-27-11.png?raw=true)
7. Where is the HEAD Pointer now?  
  - Ans. 
      1. // refer the above screenshots
## Instructions - 4: 
  ## - Work on Bug Fix on high priority on a Separate branch1.
  1. Meantime, A new high priority BUG fix work is assigned to
Programmer B who comes in second shift. As he is still working
on the file PRG2 he does not want to commit the changes done
in this file . However, he is asked to complete the BUG Fix job
ASAP.
  - Ans. 
      1. git checkout Branch2
      2. git stash // for the file that he is editing
2. He uses the Git commands and saves existing work and takes
up new BUG FIX job in PRG3 file in Branch3
  - Ans.
      1. git checkout Branch3
      2. touch PRG3.txt
      3. git add PRG3.txt
      4. git commit -m "PRG3 file has been commited"
3. After he commits the bugfix file, he gets back to his previous
work in <Branch2> file PRG2 and completes the work by adding
a few lines to PRG2
  - Ans. 
      1. git checkout Branch2
      2. git stash pop
      3. git add PRG2.txt
      4. git commit -m "PRG2 edited"
4. He also pushes the latest version to Git Hub repository and
checks it on Git hub for the new file PRG3 and new Branch3
  - Ans. 
      1. git push origin Branch3
5. <Take the snapshot (SCREEN SHOT 4) of the Repository after
Bug fix having the Git commands executed for the above steps>
  ![s_4](https://raw.githubusercontent.com/pskarthick15/Devops-1/main/Screenshot%20from%202021-08-26%2017-49-29.png)
  
## Instructions - 5 :
  1. After the Bug Fix issue is resolved by evening Shift Programmer
B , Programmer A decides to work on Branch2. He used some
additional Git commands and achieved the expected State.
  - Ans.
       1. git checkout Branch2
       2. git fetch origin 
       3. git merge origin/master Branch2
       4. // instead of  doing 2 and 3 use git pull origin
2. He makes changes in PRG2 but then he realizes the filename
should be all lowercase. He Change the filename to lower
case letter a prg2 and completes the Commits. He merges
with main branch and pushes the main to remote Git Hub
repository
  - Ans. 
      1.  git PRG2.txt prg2.txt
      2. git add prg2.txt
      3. git commit -m "Commited file after renaming"
3. <Take the snapshot (SCREEN SHOT 5) of the screen with the
Git commands executed for the above steps>
  ![s_5.1](https://github.com/pskarthick15/Devops-1/blob/main/Screenshot%20from%202021-08-26%2017-54-46.png?raw=true)
  ![s_5.2](https://github.com/pskarthick15/Devops-1/blob/main/Screenshot%20from%202021-08-26%2017-58-19.png?raw=true)
  
## Instructions - 6 : Rewrite the commit date
1. Now the coding work got completed but with one week delay
in the Project plan. Hence, Programmer A was asked to change
the date of the last commit before submitting the final local
copy of main branch to Git hub repository. He does the date
changes in the files.
  - Ans.
      1. git filter-branch --env-filter \
    'if [ $GIT_COMMIT = <Commit_id for which timestamp is to be changed> ]
     then
         export GIT_AUTHOR_DATE="Fri Jan 2 21:38:53 2009 -0800"
         export GIT_COMMITTER_DATE="<Date that we want to set>"
     fi'
2. While he was working on the date changes, his manager
notices a typographic error in prg2.
   - Ans. 
       1. // Manually change the typographic error.
3. Manager instructs him to revert back to the first version of the
commit he has just amended on prg2 .
   - Ans. 
       1. git revert <reqired commit_id from which we want to revert to>
4. Programmer A performs the undoing of the commit and
retrieves the first version of prg2, modifies and commits the file
in local branch.
   - Ans. 
       1. git reset --soft HEAD~1
       2. // modify the file in the branch and perform commit
5. He checks the history by using some Git commands .
   - Ans. 
       1. git log
6. <Take the snapshot of the screen (SCREEN SHOT 6) with the
Git commands executed for the above steps>## 
      ![s_6.1](https://github.com/pskarthick15/Devops-1/blob/main/Screenshot%20from%202021-08-26%2018-14-55.png?raw=true)
      ![s_6.2](https://github.com/pskarthick15/Devops-1/blob/main/Screenshot%20from%202021-08-26%2018-26-56.png?raw=true)
      ![s_6.3](https://user-images.githubusercontent.com/53825361/130993859-dffb1df3-bfb4-4732-bdff-314b2aae5be5.png)
      ![s_6.4](https://github.com/pskarthick15/Devops-1/blob/main/Screenshot%20from%202021-08-26%2018-26-56.png?raw=true)
  
 ## Instruction 7:
1. Programmer A combines all the programs in main branch.
  - Ans. 
       1. git checkout main
       2. git merge Branch1 Branch2 Branch3
2. He deletes all the other branches except main branch and bug
fix branch.
   - Ans. 
      1. git branch -d Branch1 Branch2
3. Programmer now stores the latest version to remote Git hub
repository suing Git commands
  - Ans.
        1. git push origin main
4. <Take the snapshot of the screen (SCREEN SHOT 7) with the
Git commands executed for the above steps> 
  - Ans. //yet to be taken
  
  ## Instruction 8 : Pushes the existing state of development work in a private remote server directory for integration Testing
1. Programmer A creates a README.md file and updates all the
Git commands used so far and puts on a remote private server
machine for future use.
  - Ans.
       1. //The one you are reading is the same
2. He pushes the latest version of the Bug fix branch and master
branch separately.
  - Ans.
       1. git push origin master
       2. git push origin Branch3
3. He verifies the Remote private server directory by logging to
the remote Private server using his credentials.
  - Ans. 
        1. //logged and checked
4. <Take the snapshot of the screen (SCREEN SHOT 8) with the
Git commands executed for the above steps>
  - Ans. Yet to be taken
5. Draw the complete workflow of all the commits
diagrammatically.  
  - Ans. Yet to be done
  
  
  
    
  
  
