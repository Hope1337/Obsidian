Docker hỗ trợ tạo nhiều loại mạng ảo

#### Bridge
- Đây là mạng mặc định, ngoài ra người dùng cũng có thể tạo mạng loại bridge này. Docker sẽ cung cấp một subnet mask cũng như tạo virtual ethernet. 
- Lưu ý, để host có thể truy cập vào được services trên port mà container cung cấp, cần có port mapping

#### Host
Dùng cho mạng với host

#### Mac VLAN
Kết nối container trực tiếp với switch, tức là giống như một thiết bị độc lập, có ip riêng. Tuy nhiên cần lưu ý về **promiscuous** mode


Thôi mệt quá ko xem nứa=)))))