
```
git log --oneline
```
- One commit per line 
- The first 7 characters of the SHA 
- The commit message



```
git log --stat
```
 - The modified files 
 - The number of lines that have been added or removed 
 - A summary line of the total number of records changed
 - The lines that have been added or removed
 


```
git log --patch 
git log --p
```
- Modified files 
- The location of the lines that you added or removed 
- Specific changes that have been made



```
git log --graph
git log --graph --oneline
```
- ASCII graph representation of the commit history
- `--graph --oneline` Compact representation of the commit history



```
git log --after="yy-mm-dd" 
git log --after="21 days ago"


git log --after="2023-11-01" --before="2024-11-08"
```
- Command will display all the commits made after the given date 
- We can also pass the applicable reference statement like "yesterday", "1 week ago", "21 days ago" and more.

- To track the commits that were created between two dates, pass a statement reference --before and --after the date



```
git log --author="ABID066"
```
- Filter the commits by a particular use



```
git log --grep="Version 3"
```
- Filter the commits by the commit message