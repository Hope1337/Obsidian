Nhớ cái màn hình chọn boot vào win hay ubuntu chứ?
Phải rồi, custom được đấy=))))

 
##### 1. Lựa chọn đi
Trước tiên thì [shopping trước đã](https://www.gnome-look.org/browse?cat=109&ord=latest), download về rồi giải nén ra

###### 2. Dời vào boot
```shell
sudo mkdir -p /boot/grub/themes
sudo cp -r Elegant-wave-blur-left-light /boot/grub/themes/
```

###### 3. Chỉnh config
```shell
sudo nano /etc/default/grub
GRUB_THEME="/boot/grub/themes/Elegant-wave-blur-left-light/theme.txt"
sudo update-grub   # Ubuntu/Debian
```

Rồi reboot thôi. 

Có thể custom nhiều cái lắm, cơ mà nghiện đấy, làm đỡ cái này thôi=))))))))

