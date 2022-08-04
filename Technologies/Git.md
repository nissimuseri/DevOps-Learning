# Git

Git is a free open source distributed version control system(VCS), that tracking changes in a filesystem.

The most using of Git is for collaboration between developers, for building a software from multiple machines to one source of truth.

Git was authored by Linus Torvalds, as a side project for developing the Linux kernel. The main goal of Git is the speed, efficiency, data integrity, and support for distributed.

Git, as a Source Control Management(SCM), is based on a popular data structure - Directed acyclic graph(DAG) - for more details see the Computer Science [definition](https://en.wikipedia.org/wiki/Directed_acyclic_graph).

This DAG has 4 basic objects and other definitions that it nice to know:

1. Blob
2. Tree
3. Commit
4. Tag

5. Ref
6. Pack

## **Git Objects:**

1. Blob
    - Includes SHA1 that is a presentation of only the content data inside the files.
    (metadata is not part of the SHA1)
    - When you change a piece of data, the SHA1 changed, so the Blob changed.
    - Each Blob contains the entire files(compressed, but not only the diff from the previous Commit).
    - The Blob is global for the repository.
    It cannot be that will be 2 Blobs with the same SHA1.
2. Tree
    - Tree contains one or more Blobs.
    - When we create an empty directory, there's no Tree that created, just we we add at least 1 file inside it.
    - Each tree contains a table of data.
3. Commit
    - Commit is a snapshot of the current state of the files(Blobs & Trees).
    - Each commit must point to the parent Commit, at least one parent(but can be more on a merge operation).
    - When a commit stay without any parent, the Garbage Collector will remove it.
4. Tag
    - A hard tag, using for create a label for commit
5. Ref
    - The Refs directory is used for a local usage.
    It includes tag names and branches(commit names).
6. Pack
    - Git in its logic create a pack on some periods or connection with the remote, for managing the data better.
    

## **Git Commands**

`git init` - initialize the current directory to be a git repository

`git status` - see the changes from the last commit

`git log` - see the commits log

`git log —graph —pretty` - a nice way to see the commits log

`git add <file_name>` - add <file_name> to the file that indexed

`git add .` - add all files in the current directory

`git commit -m <message>` - create a new commit with the string message <message>

`git checkout <branch_name>` - checkout to branch <branch_name>

`git checkout -b <branch_name>` - create a new branch with the name <branch_name> and checkout to it

`git merge <merged_branch>` - merge the `<merged_branch>` to your current branch.

`git rebase` - take the commits from your branch to the continue of the commits of other branch.

`git diff` - find the changes between to commits/branches

`git fetch` - fetch the changes from the remote branch

`git push` - push the changes from your local branch to the remote branch(origin)

`git pull` - a combination of git fetch & git merge, to pull the changes from the remote branch to your local

`git branch` - list the local branches

`git branch -a` - list all branches

`git branch -d` delete a branch(ref, not history)

`git branch -D = force delete branch` for delete branch when there is no ref for the latest commit.

### git merge VS git rebase

[TBD]

## Git Concepts

### garbage collector

Scheduled process that auto delete branches with ref count=0.

### bare repository

a repository with only `.git` folder.

### fast forward

Merge without changes on one branch.

### no ff

Merge without a fast forward method