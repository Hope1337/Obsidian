Ventoy giúp tạo usb boot từ nhiều file `.so` khác nhau.

**Steps:**
- Download ventoy + format usb.
- Installing ventoy into usbb (`Ventoy2Disk`)
- Copy file `.iso` vào là ok rồi.

#### Một số lưu ý

- Might be sẽ lỗi đối với secure boot, if it's the case, check out [this article](https://www.ventoy.net/en/doc_secure.html)
- Có hiển thị cấu trúc folder (thay vì only `.iso`) bằng `F3`
- Nếu Ubuntu có lỗi màn hình xanh, vào `sudo nano /etc/gdm3/custom.conf` và đổi `WaylandEnable` thành `True`, sau đó `sudo systemctl restart gdm3`

#### Tạo theme cho ventoy

- Tải theme [tại đây](https://www.gnome-look.org/browse?cat=109&ord=latest)
- Vào usb ventoy, tạo `ventoy`, `ventoy/themes` và `ventoy/ventoy.json` (nhớ bật show extension). Giải nén và copy folder (direct parent của `theme.txt`) vào `ventoy/themes`. Nội dung file `.json` là: 

```json
{ 
     "theme":  {
            "file": "/ventoy/themes/[name]/theme.txt"    
      }
}
```
Với `[name]` thay bằng tên theme vừa tải (tên folder).