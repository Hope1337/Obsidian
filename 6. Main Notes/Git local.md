#Git 
Như mình đã trình bày trong [[Git Introduction]], git là một distributed vcs, tức là mỗi máy đều sẽ có một version riêng của lịch sử thay đổi code. Nó sẽ lưu trữ các thông tin về lịch sử thay đổi code trong máy của bạn, locally. Không cần internet thì bạn vẫn có thể thao tác trên cái history này được và khi nào cần thì mới push lên drive service (như **git hub**) thôi. Trong bài viết này mình sẽ chỉ đề cập đến việc làm việc với history một cách cục bộ thôi chứ chưa động gì đến remote repo (git hub), tuy nhiên phần này là kiến thức nền tảng bởi vì bạn cần làm việc với history local trước khi push nó lên remote.

#### 1. Khởi tạo một repo

Trước tiên ta cần thông báo với git folder hiện tại là một repo, tức là để git track folder này:
```bash
git init
```
Lệnh này sẽ tạo ra một folder `.git` trong folder hiện tại (mặc định thì bị ẩn đi nên bạn không thấy). Folder `.git` này là nơi để git lưu trữ nhiều thứ như history của commit, branch... nên là chỉ đụng vào khi bạn biết rõ mình đang làm gì. 

#### 2. Cấu trúc của git

Mình sẽ gọi tắt toàn bộ lịch sử phiên bản của code bằng history và thao tác đẩy code vào lịch sử chỉnh sửa là `commit` nha.

Có thể hiểu git chia làm bốn vùng như sau:
- **Working directory**: folder nơi bạn đang làm việc
- **Staging area**: Nơi nháp các thay đổi trước khi thực sự lưu nó vào history
- **.git**: folder history cục bộ, có thể hiểu đây là "máy chủ cục bộ".
- **remote**: lưu history trên drive, tiện để chia sẻ

Đối với bài viết này, ta chỉ cần quan tâm đến ba vùng đầu tiên.

Khi bạn mở folder code của bạn lên và code, mọi thứ đều chỉ đang diễn ra ở **working directory**  thôi, tức là lúc này history của bạn vẫn chưa cập nhật gì cả.

Giả sử khi bạn vừa code xong một tính năng, bạn muốn đưa nó vào history:
- Đầu tiên bạn cần đưa những file code mà bạn cần lưu lại vào **staging area**, nơi này để các bạn làm nháp, tức là bạn có thể chỉnh sửa, thêm bớt các file mà bạn muốn đưa vào history trước khi thực sự làm vậy.
- Tiếp theo bạn mới thực sự đẩy mọi file trong **staging area** vào history. Bạn có thể bỏ qua **staging area** mà đẩy toàn bộ vào history hết luôn, tuy nhiên thường thì mọi người sẽ không làm vậy. Sau khi thực hiện bước này, history sẽ được thực sự lưu vào `.git`

#### 3. Các trạng thái của một file trong git

Đầu tiên, chúng ta sẽ cần phải hiểu về các trạng thái của một file trong git:

- **Untracked**: File chưa được Git theo dõi, tức là chưa được thêm vào staging area hoặc chưa được commit. Thường là các file mới tạo.
- **Tracked**: File đã được Git theo dõi. Các file này thuộc một trong các trạng thái con sau:
    - **Unmodified**: File không có thay đổi kể từ lần commit cuối cùng.
    - **Modified**: File đã được chỉnh sửa nhưng chưa được đưa vào staging area.
    - **Staged**: File đã được chỉnh sửa và được thêm vào staging area (bằng lệnh git add), sẵn sàng để commit.

Hơi nhiều thông tin, nhưng các trạng thái này rất quan trọng và nó sẽ còn xuất hiện ở những phần bên dưới.

#### 4. Làm việc với staging area


![[lifecycle.png]]


Các lệnh của git sẽ làm thay đổi trạng thái của các file, điều này là rất quan trọng để lưu ý bởi vì:

==Đối với tracked file==: 
- Giả sử bạn không add file này vào staging area hoặc đã add nhưng sau đó gỡ file này ra khỏi staging area (nhưng vẫn giữ nguyên trạng thái tracked) thì ở lần commit tiếp theo, file này vẫn xuất hiện trong commit nhưng ở version cũ chứ không bị bỏ đi.

==Đối với untracked file==:
- git sẽ không theo dõi các file này. Nếu trước đó nó là tracked file và được commit, nó vẫn được lưu trong history, tuy nhiên nếu bạn untrack file này, sau đó add và commit lần nữa, nó sẽ bị xóa khỏi history ở lần commit hiện tại. Tức là nếu một người checkout (clone code) tại thời điểm này, họ sẽ không thấy file untrack ở đâu, nhưng nếu clone code ở thời điểm trước đó thì vẫn sẽ thấy.

Vậy cho nên lưu ý về trạng thái của các file nếu muốn hành vi của git hoạt động như ý muốn của bạn.

##### a. Thêm file vào staging area

Khi bạn code, tất cả những gì diễn ra vẫn chỉ đang ở **working directory**. Giả sử bạn đã code xong một feature gì đó và muốn đưa nó vào history của bạn. Trước tiên bạn cần đưa nó vào staging area bằng cách:
```bash
# add a specific file
git add file_name.py

# add multiple files
git add file1.py file2.cpp file3.cu

# or regex
git add *.py

# or add whole working directory recursively
git add .
```

Lệnh add này chuyển các file được add thành trạng thái tracked và trong staging area của các bạn, những snapshot của các file được add sẽ được lưu lại. 

```bash
git commit -m "finish feature 1"
```

Lệnh trên sẽ đẩy tất cả những gì được staged vào trong history. Cờ `-m` là để bạn thêm message, tức là giải thích commit đó đang làm gì, message là bắt buộc. Nếu bạn chỉ dùng `git commit` git sẽ hiển thị một editor để bạn nhập nội dung (nếu nó quá dài để dùng `m`). Lưu ý là sau khi `commit`, **staging area vẫn còn lưu các snapshot đã được add** chứ không phải xóa bỏ nó đi. 

==Note==: Lệnh `git status` cho phép bạn xem được trạng thái hiện tại.

==Note==: Bạn có thể commit thẳng mà không cần thông qua staging area, tuy nhiên chỉ dùng khi bạn biết bạn đang làm gì:
```bash
git commit -am "message"
```
Cờ `a` để tự động thêm các file đã được track (đã được add trước đó)

##### b. Xóa một file khỏi staging area

Có hai kiểu remove một file khỏi staging area.

**Kiểu thứ nhất**:
```bash
git restore --staged <file>
# or
git reset <file>
```

Lệnh này chỉ gỡ file được chỉ định ra khỏi staging area chứ không đụng gì đến nội dung bên trong của file đó cả. Hơn nữa, ==file này vẫn sẽ ở trạng thái **tracked**==.

**Kiểu thứ hai**:

```bash
git rm --cached <file>
```

Lệnh này không những xóa file ra khỏi staging area mà còn ==đưa file này về trạng thái **untracked**==.

