==Mục tiêu==: Tạo một server để sync dữ liệu cho nhiều thiết bị cá nhân (chỉ cá nhân thôi nhé, không phục vụ cho team hoặc quy mô to hơn).

Mình định nêu từng bước từng bước quá trình mình thực hiện, tuy nhiên vậy thì dài quá cho nên mình sẽ tiếp cận theo một hướng khác ngắn gọn hơn

## `Nextcloud`

`Nextcloud` là một công cụ open source free cho các anh em muốn tự host một server cloud cho riêng mình, chỉ cần pull image về là okla.

## `Zerotier`

Một vấn đề lớn khác là: *làm thế nào để các máy giao tiếp được với nhau?*

Trước đây, mình sẽ nghĩ ngay đến port forwarding, tuy nhiên điều này có một vài điểm khó khăn:
- Đa số nhà mạng mình gặp đều CG NAT, không cho port forwarding
- Dù ko bị CG NAT, địa chỉ IP của các modem đều có khả năng bị thay đổi định kì
- Khi di chuyển máy qua chỗ khác thì mọi thứ phải thay đổi, cục kì lằng nhằng

`Zerotier` mang đến một giải pháp: Cho phép tạo một mạng LAN ảo (VPN), kết nối mọi thiết bị cá nhân, dù ở đâu, như thể chúng nó đang ở trong một mạng LAN.

Nói đơn giản như thế này: Bạn vào website chính thức của `zerotier`, tạo một network, sau đó ở trên mỗi thiết bị, join vào network vừa tạo bằng cách nhập network id lúc tạo network trên web. Lúc này chỉ cần lên lại web, cấp quyền cho các device xin gia nhập là xong. Giờ đây các thiết bị của bạn đã join được vào một mạng LAN, mỗi device sẽ có 1 local IP cho riêng mình.

Rõ ràng với cách làm này, bạn sẽ không còn phải lo về port forwarding, CG NAT hay vị trí của máy bạn, rất tiện.

## Một số vấn đề

- `Nextcloud` buộc bạn phải setup trusted domain, vào volume của docker, tìm `config/config.php` để add thêm vào là được.
- Nhớ `ufw allow` port để các máy trong LAN có thể lắng nghe nhau
