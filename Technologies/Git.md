# Git

** Still a draft **

## Git Objects:

1. Blob 

	- Includes SHA1 of the content data inside the files.
	- When you change a piece of data, the SHA1 and the Blob changes.
	- A new Blob contains all the new files(not only the diff).
	- The Blob is  global - if in other repository we have 

2. Tree
	- Tree contains one or more Blobs.
	- When we create an empty directory, there's no Tree that created, just we we add at least 1 file inside it.
	- Each tree contains a table of data.

3. Commit
	- Commit is a snapshot of the current stage of the files(Blobs & Trees).
	- Each commit must point to the parent Commit, at least one parent(but can be more on merge).
	- When a commit without any parent, the Garbage Collector will remove it.
	- 
4. Tag
	- Using for create a label for commit

5. Pack
	- Git in its logic create a pack on some periods or connection with the remote, for managing the data better.

## Git References:

on Refs directory, for point an object for local usage.
includes tag names and branches(commit names).


## Git Commands:

```git branch -d```
delete a branch(ref, not history)

```git branch -D = force delete branch```
for delete branch when there is no ref for the latest commit.



garbage collector = scheduled process that auto delete branches with ref count=0.

bare repository = a repository with only .git folder.

fast forward - merge without changes on one branch
no ff - without forward

