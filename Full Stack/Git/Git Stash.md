To stash your code
```git
git stash 
//or
git stash save
```
To get the recently stash
```
git stash pop
```
To view stash list
```
git stash list 
```
**Note:** You can stash a lot of time and It'll store in order
To go to the specific stash
```
git stash apply stash@{number}
```
To drop specific stash
```
git stash drop stash@{number}
```
To drop all the stash 
```
git stash clear 
```