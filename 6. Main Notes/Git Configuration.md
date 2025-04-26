[[Git]]
#Git 
Khi lần đầu sử dụng **git**, ta cần setting một số thông tin (sẽ được đề cập sau), trong bài viết này mình sẽ chỉ đề cập đến một vài thứ mình hay sử dụng, tất nhiên là còn nhiều cái khác nữa nhưng thực sự thì mình cũng không dùng tới mấy setting đó nhiều nên không biết:v

Btw, sẽ có nhiều setting nữa mà mình cũng hay dùng, tuy nhiên nó liên quan tới các chủ đề khác và khá khó để giải thích trực tiếp ở đây. Chính vì vậy mình sẽ chỉ nêu sơ lược nó là gì rồi link nó đến một bài viết khác.

Chúng ta có thể thiết lập các setting ở ba mức độ theo thứ tự giảm dần phạm vi như sau:
- **system**: áp dụng cho toàn bộ user trên máy hiện tại, file config nằm ở `usr/local/git/etc/gitconfig`
- **global**: áp dụng cho toàn bộ repo của user hiện tại, file config nằm ở `$home/.gitconfig`
- **local**: áp dụng cho repo hiện tại, file config nằm ở `.git/config`

Đối với thiết lập, **system**. Nếu bạn đã quên (như mình) thì dù là trên window hoặc linux, mỗi máy tính đều có thể dùng được cho nhiều user, tức là tùy chọn này sẽ áp dụng cho bất kì ai đăng nhập vào trong máy hiện tại. Chúng ta thường sẽ dùng `--global` lnhiều nhất.

==Một số lệnh liên quan tới file config của git==:
- Liệt kê các config (kèm với phạm vi) 
```bash 
git config --list --global
```
- thêm cờ `--edit` để chỉnh sửa file config

#### 1. Thiết lập tên và email
```bash shell
git config --global user.name "manh"
git config --global user.email randomemail@gmail.com
```
config này dùng để đánh dấu xem commit đó là của ai, tức là khi bạn tạo commit thì git sẽ gắn tên và mail vào commit đó, ví dụ:
```
commit 38ad4cd59a4...
Author: manh <randomemail@gmail.com>
Date:   Fri Apr 25 2025
```

Tất nhiên là người khác có thể fake thông tin commit để đẩy commit lên vì git sẽ không xác thực mà chỉ dùng thông tin mà bạn tự khai báo. Để git xác thực, có thể dùng **GPG-signed commit**, cái này mình chưa dùng bao giờ. 

#### 2. Thiết lập editor mặc định 
```bash shell
git config --global core.editor "code --wait"
```
> config này làm gì vậy?

Thường khi git yêu cầu nhập thông tin, nó sẽ mở một trình soạn thảo để bạn nhập (mặc định là nano trên ubuntu thì phải). Ví dụ như khi `git commit` mà không có cờ `-m` thì git sẽ bật một editor lên và yêu cầu bạn nhập nội dung vào. `code` ở trên là vscode và `--wait` là để chờ bạn đóng cửa sổ edit thì mới tiếp tục. Đừng lo nếu bạn chưa hiểu `git commit` là gì, điều quan trọng bạn cần nắm ở đây là sẽ có những lúc git cần bạn nhập thông tin, và việc config `core.editor` giúp bạn lựa chọn bạn muốn làm việc trên editor nào.  Bạn có thể setting các editor khác như:

```bash
git config --global core.editor "vim"
```
```bash
git config --global core.editor "nano"
```

#### 3. Thiết lập handle end of line

Vấn đề xảy ra khi có hai hệ thống nhận diện kí tự xuống dòng một cách khác nhau, ví dụ:
- Trên window, end of line là `\r\n`
- Trên linux/macOS, end of line là `\n`

Ví dụ **Lucky** đang code trên window còn **Min** đang code trên linux, và họ đang làm việc trên cùng một repo:

- **Lucky** muốn khi push code lên, `\r` phải được loại bỏ. Ngược lại khi clone code về, kí tự `/r` được tự động thêm vào.
- **Min** muốn khi push code lên, nếu `\r` được vô tình thêm vào (có thể do editor) thì nó phải được loại bỏ.

Để đạt được các hành vi trên, mình sẽ setting `core.autocrlf` (`crlf` là viết tắt của carriage return `cr`, và line feed `lf`) bằng một trong ba giá trị:
- **true**:
    - Khi check out: Chuyển tất cả `\n` thành `\r\n` trên Windows.
    - Khi check in: Chuyển tất cả `\r\n` thành `\n` trong repository.
    - Phù hợp cho người dùng Windows làm việc với kho lưu trữ dùng \n.
- **input**:
    - Chỉ chuyển `\r\n` thành `\n` khi check in, không thay đổi khi check out.
    - Thường dùng trên Linux/macOS.
- **false**:
    - Không tự động chuyển đổi EOL, giữ nguyên ký tự nào có trong tệp.
    - Dùng khi bạn muốn kiểm soát thủ công.

```bash
git config --global core.autocrlf input
```

#### 4. Default branch name

```bash
git config --global init.defaultBranch main
```
Mặc định branch là master, tuy nhiên quy ước này đã thay đổi, tức là giờ đây tên của branch chính nên được đặt là `main`. 

#### Tạo alias

Nếu có một lệnh nào đó mà bạn dùng nhiều thì có thể tạo alias cho nó, ví dụ:
```bash
git log -1 HEAD
```
Bạn có thể alias lệnh này bằng cách:
```bash
git config --global alias.last 'log -1 HEAD'
```

#### Check config mặc định
```bash
git config --list
```

