##### 1. Quên chưa sudo
Vừa nhập lệnh gì đó nhưng quên `sudo`? Thay vì gõ `sudo` và lại gõ lại cái lệnh vừa nãy thì đơn giản là dùng
```shell
sudo !!
```

##### 2. Quay lại directory trước đó 
```shell
cd - 
```

##### 3. Xóa màn hình
```shell
Ctrl + L
```
Không hẳn là xóa màn hình mà là chuyển dòng hiện tại lên đầu, giống như xóa như vẫn còn lịch sử (nếu lăn chuột lên xem)

##### 4. Đặt mốc để quay lại directory chỉ định
```shell
pushd /etc #ghi nhớ directory này
popd #quay lại directory trong bộ nhớ
```

##### 5. Tạo cột cho dễ nhìn
```shell
[command] | column -t #t for table
```

##### 6. Translate
```shell
tr a-z A-Z
hello
HELLO
```
đổi tập kí tự này sang tập kí tự khác


---

#### Edit command line
- Ctrl-a : move the cursor to the beginning of the line
- Ctrl-e : move the cursor to the end of the line
- Ctrl-b, Alt-b: move backward, by one char or by one word
- Ctrl-f, Alt-f: move forward, by one char or by one word
- Ctrl-d : delete character under the cursor
- Ctrl-k : delete everything from the cursor till the end of the line
- Ctrl-u : delete the entire line
- Alt-d : delete till the end of the current word

#### Job control
- Ctrl-c: Terminate (kill) the running job
- Ctrl-z: Stop the running job (you can then put in background with `bg`)
- Ctrl-d: Terminate the Shell and close the Terminal
- Ctrl-l: Clear the terminal
- Ctrl+s: Stop all output to the screen. This is particularly useful when running commands with a lot of long, verbose output, but you don't want to stop the command itself with Ctrl+C.
- Ctrl+q: Resume output to the screen after stopping it with Ctrl+S.

#### Copy file giữa remote và local
```shell
scp -r localdir remotelogin@remotecomputer:remotedir
rsync -avh localdir/ remotelogin@remotecomputer:remotedir
```

