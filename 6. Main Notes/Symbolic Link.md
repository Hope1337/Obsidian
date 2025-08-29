Trong linux, có ba thành phần chính để quản lý dữ liệu
- Directory entries: gồm inode id và tên file: như kiểu một danh bạ tra cứu
- Inode id: chứa các meta data cũng như địa chỉ vùng nhớ chứa dữ liệu thực sự
- Data block: dữ liệu thực sự của file sẽ được lưu ở đây

==Note==: Số lượng inode là có giới hạn, do đó nếu bạn check thấy linux báo không còn storage mà lúc check ổ đĩa vẫn trống thì khả năng cao là do hết inode.

> Từ từ đã, symbolic link là gì?

Còn nhớ trên window, bạn tạo shortcut cho các app, để nó ngoài desktop cho dễ nhìn không? Đó là một dạng link.

```shell
ln [command] [target_file] [link_name]
ln a.txt b.txt #hard link
ln -s a.txt c.txt #sim link
```

```shell
manh@manh-laptop:~$ ls -li
total 8
49552 -rw-r--r-- 2 manh manh 3 Aug 27 15:32 a.txt
49552 -rw-r--r-- 2 manh manh 3 Aug 27 15:32 b.txt
49307 lrwxrwxrwx 1 manh manh 5 Aug 27 15:41 c.txt -> a.txt
```

hard link sẽ dùng chung inode trong khi symbolic link như một object riêng biệt và có inode riêng

==Note==: Dùng absolute path để tránh lỗi ngu.