
## `dkpg`

- `dkpg` là trình quản lí packages cấp thấp, dùng để cài đặt `.deb`
```shell
sudo dpkg -i [.deb]
```
`dkpg` không tự động resolve các dependency, tức là thiếu gì thì phải tự cài.

---
## apt

`apt` là phiên bản xịn hơn của `dkpg`, có thể tự động resolve môi trường luôn:
```shell
sudo apt install [.deb]
```
```shell
sudo apt upgrade #just update current packages to newer version, ignore ones that require others to changes.
sudo apt dist-upgrade #force update (stronger version compared to above)
```

- file `/etc/apt/sources.list` chứa các sources mà apt sẽ dùng để install packages

```shell
sudo apt remove [package] #retain user data
sudo apt purge [package] #completely remove
```