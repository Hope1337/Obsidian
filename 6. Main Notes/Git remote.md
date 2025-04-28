#Git 
#### 1. Thiết lập kết nối local và remote

```bash
git remote add pb https://github.com/paulboone/ticgit
```

Lệnh này thiết lập kết nối giữa repo local và repo remote. Có thể thiết lập nhiều remote khác nhau cho cùng một repo local:
```bash
git remote add origin https://github.com/your-username/repo.git
git remote add upstream https://github.com/original/repo.git
```

Kết quả của `git remote -v`:

```bash
origin https://github.com/your-username/repo.git (fetch) 
origin https://github.com/your-username/repo.git (push) 
upstream https://github.com/original/repo.git (fetch) 
upstream https://github.com/original/repo.git (push)
```

### 2. Một số lệnh hữu ích
```bash
git remote show origin
git remote rename pb paul
git remote remove paul
```
