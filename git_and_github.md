# Git and GitHub Notes

## What is Git ?

- Distributed Version control system that helps developers track their codebases, collaborate with others and manage multiple versions of a project.
- Git is decentralized , which means doesn't need a central server. Instead every dev has a full copy of repo on their machines.
- git is a must have skill for all devs.

### Key features of Git 

1. Distributed
2. Version tracking
3. Collaborative
4. Branching
5. Merging
6. Remote Repos
7. Extensive Tooling
8. Staging Area
9. Open Source and Free

## What is GitHub

- Web based Platform used for version control and collab. It hosts repos and provides an interface to manage your codebase and do many other imp things. 
- GitHub offers collab features like bug tracking, feature request, task management and wikis.

### Git is a **VCS [version control system]** and GitHub **hosts repos**

## Configure Git:

```bash
git config --global user.name "username"
git config --global user.email "useremail@gmail.com"
```
## Git Workflow

1. `git init` [ Initialize a new repo - this creates a hidden file `.git`]
2. `git add .` [ Add your files to staging area ]
3. `git commit -m "message"` [ Commit your files to local repo ]
4. `git push` [ Push your files to remote repo ]

### other common commands :

5. `git pull` [ Pull changes from remote repo to your local machine - ***very when working in a team*** ]
6. `git clone` [ Clone an entire remote repo to your local machine ]

## Branching and Merging 

- Branching allow you to work on new feature or bugs without affecting the main codebase.
- When ready you can merge the new code into main codebase (*branch*)