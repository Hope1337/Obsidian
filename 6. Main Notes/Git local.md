Như mình đã trình bày trong [[Git Introduction]], git là một distributed vcs, tức là mỗi máy đều sẽ có một version riêng của lịch sử thay đổi code. Tức là nó sẽ lưu trữ các thông tin về lịch sử thay đổi code trong máy của bạn, locally. Không cần internet thì bạn vẫn có thể thao tác trên cái history này được và khi nào cần thì mới push lên drive service (như **git hub**) thôi.

#### 1. Khởi tạo một repo

Để thông báo với git folder hiện tại là một repo, tức là để git track folder này:
```bash
git init
```
Lệnh này sẽ tạo ra một folder `.git` trong folder hiện tại (mặc định thì bị ẩn đi nên bạn không thấy). Folder `.git` này là nơi để git lưu trữ nhiều thứ như history của commit, branch... nên là chỉ đụng vào khi bạn biết rõ mình đang làm gì.