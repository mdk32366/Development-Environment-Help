### Development-Environment-Help

####Git Command Line Crash Course:

See current changes: *git status*

Create a new branch: *git checkout -b my-branch*

Merge branch: *git checkout branch-to-merge-into then git merge my-branch --no-ff*

Add your changes: *git add path/to/your/file or git add .*

Undo adding changes: *git checkout -- path/to/file*

Commit changes (after adding): *git commit -m "your helpful commit message"*

Undo commit (put changes back to staging): *git reset --soft HEAD^*

Push changes to server: *git push origin my-branch*
