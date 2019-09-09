# Git Commands

## 1. delete merged branches from local:

```bash
git branch --merged master | grep -v 'master$' | xargs git branch -d
```

> taken from <http://stackoverflow.com/questions/13064613/how-to-prune-local-tracking-branches-that-do-not-exist-on-remote-anymore>

## 2. copy file from one repository to another, with history

```bash
cd /path/to/source_repo
git log --pretty=email --patch-with-stat --reverse -- path/to/fileOrDirectoryToCopy | (cd /path/to/destination_repo && git am)
```

> taken from <http://gbayer.com/development/moving-files-from-one-git-repository-to-another-preserving-history/>
