### Docker context

Chuyển qua dùng `default` context, đây chính là native docker và chạy trực tiếp trên nhân linux, trong khi đó docker desktop lại dùng một máy ảo, do đó không kết nối mạng được với host.
```shell
docker context ls
docker context use default
```

---

### Shared memory
Docker tối ưu bằng cách nếu cả hai cùng nằm trên mạng host: "Ơ, hai đứa mình cùng nằm trên một máy tính vật lý mà? Gửi qua mạng làm gì cho chậm, dùng **Shared Memory (SHM)** đi!". Và cũng do vấn đề về quyền truy cập file nên các ông này không lắng nghe được nhau, do đó tắt SHM đi và dùng giao thức UDPv4

```shell
export FASTDDS_BUILTIN_TRANSPORTS=UDPv4
```


--- 

# WSL2 + Docker + ROS2

- **Quan trọng nhất**: mở port ra (`sudo ufw`), z thì nó mới giao tiếp với nhau được. Đm quên mất cái này làm cả buổi không xong=))))

- Tạo file `.wslconfig` trong `C:\Users\[name]` với nội dung sau:
```shell
[wsl2]
networkingMode=mirrored
firewall=false
```
Này là để wsl2 xài cùng mạng với máy host, hơn nữa tắt tường lửa để khỏi setup port.

- Nếu không được nữa thì xem xét lệnh này:
```shell
Set-NetFirewallHyperVVMSetting -Name '{40E0AC32-46A5-438A-A0B2-2B479E8F2E90}' -DefaultInboundAction Allow

Set-NetFirewallHyperVVMSetting -Name '{40E0AC32-46A5-438A-A0B2-2B479E8F2E90}' -DefaultOutboundAction Allow
```

Này có nghĩa là cho luồng ra luồng vào máy ảo (ID máy ảo nằm trong ngoặc), cơ mà theo gemini thì không cần set.