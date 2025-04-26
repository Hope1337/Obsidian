#Git

Mình định viết một chuỗi bài về git cơ. Cơ mà nó vừa dài, vừa chán, vừa không có cấu trúc, kiểu như mấy cái lệnh nó cứ dính với nhau á, tách biệt ra thì không được mà viết chung thì nó rối rắm nên thôi=))))). Cái lệnh nào mình hay dùng thì mình vứt vào chung một chỗ luôn.

```bash
# set up stream
git push -u origin main

# add remote
git remote add 

# 
git status
git status -s --branch

# diff working dir vs staging
git diff

# staging vs commit
git diff --cached

# Stops tracking file.txt but keeps it in the working directory
git rm --cached file.txt

# save current changes
git stash
git stash push -m "ghi chú tùy chọn"

# list previous stash
git stash list

# revert the previous changes
git stash apply stash@{0} # keep the stash
git stash pop stash@{0} # apply and discard the stash

# clear stash
git stash clear
git stash drop stash@{1}
```