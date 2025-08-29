tmux là terminal multiplexer, tức là cho phép chạy nhiều phiên terminal (shell) trong cùng một cửa sổ terminal. ==Quan trọng là tmux có thể giữ cho các phiên này hoạt động ngay cả khi đóng cửa sổ terminal==, điều này rất quan trọng khi nếu bạn muốn connect vào server remote, chạy code và disconnect nhưng vẫn muốn code chạy trên remote.

## Session

- Tạo session mới:
```shell
tmux new -s [session_name]
```

- list các session hiện có:
```
tmux ls
```

- connect vào session hiện có 
```shell
tmux attach -t [session_name]
tmux a -t [session_name] #tuy chon
```

- detach phiên thì bấm `Ctrl + B + D`
- Đổi tên session hiện tại
```shell
tmux rename-session [new_name]
Ctrl + B + $ #simpler
```

- Switch giữa các session
```shell
Ctrl + B + s
```
## Panes and Splits

- Chia dọc màn hình
```shell
Ctrl + B + %
```
- Chia ngang màn hình
```shell
Ctrl + B + "
```
- Để navigate dùng `Ctrl + B` + arrows
- Để đóng pane hiện tại dùng `Ctrl + B + X`, hoặc đơn giản là gõ `exit`.

## Windows

- Tạo window mới:
```shell
tmux new-window
Ctrl + B + C #(hoặc đơn giản hơn)
# nhìn bar ở bottom để thấy window mới được thêm vào
```
- Di chuyển giữa các window
```shell
Ctrl + B + N #(next)
Ctrl + B + P #(previous)
Ctrl + B + [number] #moving based on window's number
```
- Xóa window hiện tại
```shell
Ctrl + B + &
```
- rename current window
```shell
tmux rename-window [new_name]
Ctrl + B + , #hoặc đơn giản hơn là làm như này
```


## Config

- Tạo file `/home/[user]/.tmux.conf`
```shell
set-option -g prefix C-j
set-option -g prefix2 C-f

# Load config bằng cách prefix + r
bind-key r source-file ~/.tmux.conf \; display-message "tmux.conf reloaded"

# Mouse mode để cablirate mấy cái terminal (kéo thả chỉnh size)
set -g mouse on 
# or using tmux set -g mouse on for convinence
```
 Có thể tạo 2 prefix luôn, tức là sau này cứ `Ctrl + j` với `Ctrl + f` 
- 