
Hồi cấp ba, lúc mới làm quen với `VSCode`, thứ làm mình đau đầu nhất là setting environment PATH.

> Nó là cái gì vậy?

Thực sự thì tại thời điểm đó mình không không hiểu nó là gì, mình chỉ biết việc thiết lập các đường dẫn như vậy là cần thiết, một bước bắt buộc, nếu mình muốn `VSCode` chạy được code `C++` của mình. Thật may, gần đây (cũng là năm ba đại học rồi) thì mình mới thực sự hiểu cái PATH đó là gì và tại sao phải cần nó.

Bạn còn nhớ lúc code mà cần `import` hay `#include` các thư viện bên ngoài vào chứ? Bạn có bao giờ tự hỏi nó hoạt động bằng cách nào không?

Thực ra thư viện cũng là code (hoặc file object, là code sau khi được biên dịch thành mã máy) được nằm trên máy của bạn. Việc bạn import các thư viện vào thì compiler phải đi tìm nơi chứa các thư viện này rồi chạy. 

> Tìm ở đâu?

Việc bạn set các PATH đó chính là nói cho compiler biết các thư viện đó nằm ở chỗ nào đó! Giờ thì hiểu tại sao phải setting PATH rồi chứ =))))

### 1. PATH trong Window

![[Pasted image 20250428184920.png]]

Các anh em code `C++` 24/7 chắc phải nhớ cái giao diện này rồi :v 
Các bạn có thể thấy là nó chia thành hai loại: **sytem variables** và **user variables**. Cái đầu tiên khi được set nó sẽ áp dụng cho toàn bộ user, cái thứ hai là custom cho chính user hiện tại mà thôi. Mấy cái tên một số có thể đặt tùy ý, còn một số thì không. Ví dụ trong code của các thư viện đó nếu nó quy định rằng phải lấy đúng cái PATH name đấy thì mới hoạt động thì việc bạn đổi tên là nhót, còn không thì cứ quăng đại vào là được. Cái nào quăng đại được thì hên xui, mình cũng không rõ đâu=))))))

### 2. PATH trong Ubuntu

`PATH` trong Ubuntu thì hơi lằng nhẳng một tí bởi vì nó không có UI để chỉnh như Window mà phải thông qua command line và file cấu hình. Có hai kiểu để setting PATH là tạm thời và vĩnh viễn.
##### 1. Setting PATH tạm thời

Đối với cách này, các PATH chỉ tồn tại ở terminal hiện tại, không tồn tại trong terminal khác:
```bash
export PATH=$PATH:/đường/dẫn/mới
```
Trong đó:
- `PATH=`: gán biến
- `$PATH`: biến `PATH` thực chất đã tồn tại rồi, thêm dấu `$` tức là đang lấy giá trị của biến đó
- Phần còn lại là append `PATH` của chúng ta vào `PATH` cũ, phân cách bằng dấu hai chấm

> `export` dùng để làm gì?

Nếu bạn bạn không dùng `export`, tiến trình con sẽ không thấy được biến `PATH`, [ví dụ](https://stackoverflow.com/questions/1158091/defining-a-variable-with-or-without-export):
```bash
export foo="Hello, World"
 bar="Goodbye"

echo $foo
Hello, World
echo $bar
Goodbye

bash
bash-3.2$ echo $foo
Hello, World
bash-3.2$ echo $bar

bash-3.2$ 
```

##### 2. Setting PATH vĩnh viễn

Đối với cách làm này, terminal nào cũng sẽ được áp dụng chứ không riêng gì terminal hiện tại. Đầu tiên ta mở file `bashrc`
```bash
nano ~/.bashrc
```
File này là file cấu hình, trước khi terminal chạy, nó sẽ run file này trước.
Tiếp theo ta append PATH vào cuối file:
```bash
export PATH=$PATH:/đường/dẫn/mới
```
Cuối cùng là áp dụng cho terminal hiện tại
```bash
source ~/.bashrc
```

> áp dụng cho terminal hiện tại là sao?

Tức là ta vừa đổi config, tuy nhiên terminal hiện tại chưa biết gì về việc này, vì vậy ta phair chạy lại file cấu hình lần nữa để terminal cập nhật các config vừa thay đổi.


==Note==: [Ngoài biến `PATH` ra còn nhiều biến khác nữa](https://unix.stackexchange.com/questions/44990/what-is-the-difference-between-path-and-ld-library-path):
- `PATH`: dùng cho các tệp thực thi hoặc scripts
- `LD_LIBRARY_PATH`: dùng cho thư viện (`usr/local/lib`, `usr/lib`...)
- `PYTHONPATH`: thư viện python
Và còn nhiều biến khác, mình chỉ nêu một số để ví dụ, tuy nhiên cần lưu ý là `PATH` và `LD_LIBRARY_PATH` là dùng siêu nhiều, nên nhớ.