
#### delete merged branches from local:
git branch --merged master | grep -v 'master$' | xargs git branch -d
taken from http://stackoverflow.com/questions/13064613/how-to-prune-local-tracking-branches-that-do-not-exist-on-remote-anymore
