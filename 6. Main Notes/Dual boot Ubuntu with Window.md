
Đây đây, quên quài khổ quá cơ, note lại luôn cho vừa lòng người.

Tốt nhất là format và erase disk luôn để lúc vào cài đặt nó hiện free space
![[Pasted image 20251128093815.png]]


- Đầu tiên là tạo phân vùng EPS, đây là nơi lưu các tệp tin khởi động của hệ thống (mình cũng đếch biết là gì đâu nhưng mà hiểu vậy đi), nó cần ít nhất là 100Mb, tui để 500Mb, và định dạng là VFAT.
- Tiêp đến là phân vùng Swap, tầm 8gb
- Cuối cùng là phân vùng mount, dùng tất cả bộ nhớ còn lại của free space, à ở mục mount point nhớ gõ `/`. 

---
### GRUB theme

**Bước 1**: Tải các theme ở đây:

**Bước 2**: 
```shell
sudo mkdir /boot/grub/themes/
sudo tar -xvf Stardew-Valley.tar -C /boot/grub/themes/
```

**Bước 3**: Edit `/etc/default/grub` file. Find the line starting with "#GRUB_THEME" and change it to "GRUB_THEME=/boot/grub/themes/THEME_FOLDER/theme.txt

 **Bước 4**: Run`sudo update-grub`
























