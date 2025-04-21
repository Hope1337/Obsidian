#### I. Ngữ cảnh

Thông thường khi muốn push hay clone một repo về, mình thường dùng terminal và gõ các lệnh hay gặp như git clone, git push ... và git sẽ yêu cầu xác thực người dùng. Lần đầu push, git sẽ yêu cầu mình nhập username và auth code (trước đây là password, tuy nhiên đã được loại bỏ và thay bằng auth code), git sẽ lưu thông tin này lại cho các lần sau. Tuy nhiên cũng có một cách làm khác là sử dụng **ssh key**: Thay vì dùng username và mật khẩu, bạn tạo một key (code mã hóa) trên máy bạn (cách tạo sẽ được đề cập sau), thêm key vừa tạo lên github setting, lúc này git đã có thể nhận diện được.

> Vậy sao phải cần ssh key chi? Https dễ mà gần gũi hơn mà?

Học hỏi thôi, có vẻ mấy ông dev thích ssh hơn. Hình như nếu không có sẵn credential manager (cái dùng để lưu thông tin xác thực của git) thì mỗi lần push sẽ bị yêu cầu xác thực một lần, ssh key thì config một lần thôi là được 
#### II. Cách git truyền và nhận dữ liệu

- Git sử dụng giao thức để truyền và nhận dữ liệu.
- Giao thức (protocol) là tập hợp các quy tắc, chuẩn mực hoặc quy định được thiết lập để điều chỉnh việc trao đổi thông tin hoặc tương tác giữa các thiết bị, hệ thống, hoặc thực thể trong một mạng hoặc môi trường cụ thể.
- Khi dùng `git remote add https://github.com/username/repo.git`, git sẽ ngầm hiểu đây là sử dụng giao thức https. Còn `git remote add origin git@github.com:username/repo.git` là giao thức ssh. 

#### III. Config ssh key

###### Bước 1: Kiểm tra đã có SSH key chưa
```bash
ls ~/.ssh
```
Nếu bạn thấy file như `.pub`, nghĩa là đã có key rồi. Nếu không, làm tiếp bước 2 để tạo.

###### Bước 2: Tạo SSH key
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```
-  ```-t ed25519``` : dùng thuật toán mã hóa ed25519
- ```-C``` : comment, để tên email cho dễ phân biệt key này của ai

###### Bước 3: Copy public key
```bash
cat ~/.ssh/id_ed25519_github.pub
```
Nó sẽ xuất hiện key vừa tạo, copy key này vào github (setting ->SSH and GPG keys)


###### Bước 4: Test kết nối
```bash
ssh -T git@github.com
```
Kết quả sẽ là:
```bash
Hi your-username! You've successfully authenticated...
```
Nếu nó thông báo lỗi gì đó thì chúc mừng bạn quay vào ô mất lượt=))) (debug đê)


###### Bước 5: Chuyển kho Git từ HTTPS sang SSH
Nếu git clone như thông thường thì git sẽ dùng https, muốn chuyển sang ssh thì cần một số bước
Vào thư mục chứa .git hiện tại, kiểm tra xem đang dùng https hay ssh
```bash
git remote -v
```
- Nếu thấy ```https://github.com/username/repo.git```, bạn đang dùng HTTPS.
    
- Nếu thấy ```git@github.com:username/repo.git```, bạn đã dùng SSH (bỏ qua bước này).

Chuyển remote URL sang SSH:
```bash
git remote set-url origin git@github.com:username/repo.git
```


###### (Optional) Bước 6: Thêm vào `~/.ssh/config` (nếu có nhiều key)

Nếu máy dùng nhiều key thì có thể thiết lập 
```conf
Host github.com-personal
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_personal
```

- ```Host github.com-personal``` 
	-  Đây là bí danh (alias) bạn đặt để chỉ một cấu hình SSH cụ thể.
	-  Thay vì sử dụng trực tiếp `github.com`, bạn có thể dùng `github.com-personal` trong URL SSH (ví dụ: `git@github.com-personal:username/repo.git`).
	-  Bí danh này giúp phân biệt giữa các tài khoản GitHub hoặc các khóa SSH khác nhau.

- `HostName github.com`:
	-  Chỉ định máy chủ thực tế mà SSH sẽ kết nối tới, ở đây là `github.com`.
	-  Dù bạn dùng bí danh `github.com-personal`, SSH vẫn kết nối đến máy chủ GitHub thực (`github.com`).

- `User git`:
	-  Xác định tên người dùng SSH khi kết nối đến GitHub.
	-  GitHub yêu cầu sử dụng git làm tên người dùng SSH cho tất cả các kết nối (không phải username GitHub của bạn)

- `IdentityFile ~/.ssh/id_ed25519_personal`:
	-  Chỉ định tệp khóa riêng SSH sẽ được sử dụng cho kết nối này.
	-  Ở đây, bạn đang dùng khóa riêng `~/.ssh/id_ed25519_personal` (thay vì khóa mặc định như `~/.ssh/id_ed25519`).
