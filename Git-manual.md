# config list

git config --list

# adding config

Not global

      git config user.name "name"
      git config user.email "email"

Global

      git config --global user.name "name"
      git config --global user.email "email"

# removing config

      git config --unset config.name
      git config --unset --global config.name

# useful custom commands

git log -> `git lg`

      git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

git status -> `git st`

      git config --global alias.st status

git branch -> `git br`

      git config --global alias.br branch

git checkout -> `git ck`

      git config --global alias.ck checkout

# add

add changes

      git add . 👉 Add all changes
      git add index.js 👉 Add index.js only to changes

# status

Check changed

      git status

# commit

Take a snapshot

It is possible to write a single line of commitment messages by attaching `-m "text"`.

      git commit

# push

Upload a snapshot

      git push origin master

# pull

Update local storage.

      git pull origin master

# remote

Check remote storage list

      git remote -v

Add remote storage

      git remote add url

Remove remote storage

      git remote remove

# commit amend

Overwrite commit(snapshot)

      git add .
      git commit --amend --no-edit
      git push origin master --force

# empty commit

make empty commit

      git commit --allow-empty -m "text"
      git push origin master

# reset

Recover the commit and return it to the previous state, leaving no recovery content.

Options are available : `soft`, `mixed`

      # Returns the previous commitment based on HEAD, and deletes the HEAD commit.
      git reset --hard HEAD~1

# branch

Create a new branch

      git checkout -b text

Check the branch list

      git branch

Move between branches

      git checkout branch명

Delete branch

      git branch --delete branch명
      ❗ `-D` can be forced to delete if the commit history remains. ❗

Add remote storage branch

      git push origin branch_name

Remove remote storage branch

      git push origin :branch_name

Check remote storage branchs

      git branch -r

# merge

Merge what you worked on dev branch into master branch.

      git checkout master
      git merge dev

# rm -f --cached

1. Edit `.gitignore` (i.e. add .env, ...)
2. git rm -r --cached .
3. git add .
4. git commit or git commit -m "message"
5. git push origin master
