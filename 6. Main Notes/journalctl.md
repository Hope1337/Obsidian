- linux có kha khá các processes chạy ngầm và chúng cũng output ra một file log, quản trị viên có thể xem file log này để xem điều gì đang diễn ra.
- `journalctl` giúp theo dõi các log đó
```shell
journalctl -f #auto scroll
journalctl -u [daemon_name] #u for units, focus on specific service
```


```shell
journalctl --since "2025-03-25 12:00:00"

# 
id -u [user_name]
journalctl _UID=[user_id]
```

