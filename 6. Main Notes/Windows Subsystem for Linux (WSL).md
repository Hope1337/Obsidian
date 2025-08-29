WSL giúp bạn có một terminal chạy lệnh linux trên window.


## Cài đặt WSL

#### Bật ảo hóa
Để cài đặt WSL thì trước tiên phải vào BIOS bật ảo hóa. Đây là chức năng mà cpu hỗ trợ (về cả mặt phần cứng) để chạy máy ảo. WSL 2 yêu cầu ảo hóa được active để có thể hoạt động.

Dùng lệnh `systeminfo`, check dòng:
```shell
Virtualization Enabled In Firmware: No
```
Nếu đang là `No` như trên thì cần vào BIOS để bật (cái dm mù, BIOS để AMD V ngay trước mắt mà ko thấy, mò cách bật advanced setting cả tiếng)

Dùng powershell để bật 2 tính feature cần thiết để chạy WSL lên:
```powershell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart 
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

Không thì tick thủ công bằng GUI cũng được:

Rồi rồi thì vào (trong Start):
![[Pasted image 20250824012829.png]]

Sure rằng `Virtual Machine Platform` và `Windows Subsystem for Linux` được tick.
![[Pasted image 20250824012859.png]]
#### Cài WSL
```power shell
wsl.exe --list --online #list of available distros, Ubuntu recommended
wsl.exe --install <Distro>
```

#### Bật WSL thôi
```
wsl
wsl -d Ubuntu #in case you have more than 1 distro
```
![[Pasted image 20250824011126.png]]
`/mnt` sẽ là ổ đĩa thực trong khi `/home/manh`, `/etc`, `/usr`… là các ổ đĩa ảo được lưu trữ trong thư mục riêng (chỗ nào thì không rõ=]]])

==Note==: có hai level ảo hóa là level 1 và level 2. Level 1 là chạy trực tiếp trên CPU, ví dụ như WSL2 hay Hyper-V, level 2 là qua trong gian, tức là chạy bên trong một hệ điều hành vốn đã chạy trên level 1, ví dụ VirtualBox, giả lập Android, Bluestack. ==Hai cái này xung đột lẫn nhau==, tức chỉ có thể bật một trong hai level cùng 1 lúc, bật cả 2 thì cái này chạy thì cái kia không chạy được, do đó cần dựa vào nhu cầu mà bật đúng cái cần thiết.


## Tips and tricks

- Có thể chạy các lệnh CLI trên WSL chỉ bằng cách thêm `.exe` vào cuối
- VSCode có WSL extension cho phép nối terminal vào WSL