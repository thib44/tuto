To go back to last commit : <br>
`git reset HEAD~1`

And then to remove all the modifications : <br>
`git checkout -- .`

Or for all the modifications in current commit :<br>
`git reset --hard`



## To copy a commit branch :
  - Go to the branch,
  - `git cherry-pick <hash-of-commit>`

## To delete a commit :
 `git revert <hash-of-commit>`


## Remove file which I don't want to be tracted (delete it)

`git rm -r --cached --ignore-unmatch MY_FILES`

## Diff between two files (in same branch, same commit etc)
`git diff --no-index file1 file2`
