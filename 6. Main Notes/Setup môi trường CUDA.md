Có ba thứ cần lưu ý:
- `CUDA Driver`: để máy có thể giao tiếp được với GPU, người dùng thông thường chỉ cần cài cái này để các ứng dụng có thể dùng được GPU là được.
- `CUDA toolkit`: dùng để lập trình với GPU
- `cuDNN`: dùng để lập trình với GPU, đặc biệt là mạng học sâu. Thường thì pytorch sẽ cài sẵn cái này nhưng đôi lúc không có.

## 1. Installing Miniconda

1. Download miniconda installer
```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
```

2. Install
```bash
bash ~/Miniconda3-latest-Linux-x86_64.sh
```

3. Add `conda` into `PATH`
Nếu sau khi install mà vẫn không dùng `conda` được thì khả năng là chưa add `conda` vào `PATH`. Append vào `PATH` cũng dễ thôi mà mình làm biếng với lại cái lệnh dài vl khó nhớ, thôi thì cứ tìm `.../miniconda3/condabin/conda` rồi chạy `./conda init` là được (nhớ `source ~./bashrc` lại để kích hoạt cho shell hiện tại).

==Important==: Thay vì gõ 'yes' thì hãy gõ 'Y'

```bash
source ~/.bashrc
```

4. Disable auto activate `base` env
```shell
conda config --set auto_activate_base false
```

---

## 2. Installing CUDA driver

Nếu bạn gõ `nvidia-smi` mà báo lỗi thì là chưa có driver đó.

1. Recommended drivers
```shell
sudo apt-get update
# sudo apt-get upgrade shouldn't run, too long
sudo apt install ubuntu-drivers-common
sudo ubuntu-drivers devices
```
Một list các recommended drivers sẽ hiện ra cho bạn lựa chọn:
```shell
$ sudo ubuntu-drivers devices
== /sys/devices/pci0000:00/0000:00:01.0/0000:01:00.0 ==
modalias : pci:v000010DEd00001F15sv00001028sd000009ABbc03sc00i00
vendor   : NVIDIA Corporation
model    : GA104M [GeForce RTX 3070 Laptop GPU]
driver   : nvidia-driver-470 - distro non-free
driver   : nvidia-driver-525 - distro non-free
driver   : nvidia-driver-535 - distro non-free
driver   : nvidia-driver-550 - distro non-free recommended
driver   : xserver-xorg-video-nouveau - distro free builtin
```
Nên chọn cái có dòng `recommended` trước, nếu lỗi thì đổi cái khác:D

==Quan trọng==: Ưu tiên bản driver open trước nhé, giờ recommend cài xong vẫn no devices were found, resolve cả buổi.

2. Install driver
```shell
sudo apt install nvidia-driver-xxx
sudo reboot
```

3. Verify installation
```shell
nvidia-smi
```

Optional: cài đặt tự động
```shell
sudo apt-get install -y nvidia-open #open kernel
sudo apt-get install -y cuda-drivers #proprietary kernel
```
## 3. Installing CUDA Toolkit

Nếu bạn gõ `nvcc --version` mà báo lỗi thì đang chưa có `CUDA Toolkit đó`. Vào [trang chủ chính thức](https://developer.nvidia.com/cuda-toolkit) mà cài đặt.

1. Ví dụ cài đặt `CUDA Toolkit 13.0`
```shell
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2404/x86_64/cuda-ubuntu2404.pinsudo mv cuda-ubuntu2404.pin /etc/apt/preferences.d/cuda-repository-pin-600wget https://developer.download.nvidia.com/compute/cuda/13.0.1/local_installers/cuda-repo-ubuntu2404-13-0-local_13.0.1-580.82.07-1_amd64.debsudo dpkg -i cuda-repo-ubuntu2404-13-0-local_13.0.1-580.82.07-1_amd64.debsudo cp /var/cuda-repo-ubuntu2404-13-0-local/cuda-*-keyring.gpg /usr/share/keyrings/sudo apt-get updatesudo apt-get -y install cuda-toolkit-13-0
```

2. Setup `PATH`
Nếu cài rồi mà `nvcc` vẫn không được recognized thì phải append vào `PATH` và `LD_LIBRARY_PATH`:

```shell
# open .bashrc
nano ~/.bashrc

# add the following 2 lines at the bottom
export PATH=/usr/local/cuda-13.0/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-13.0/lib64:$LD_LIBRARY_PATH
```

3. Verify
```shell
nvcc --version

# output
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2025 NVIDIA Corporation
Built on Wed_Aug_20_01:58:59_PM_PDT_2025
Cuda compilation tools, release 13.0, V13.0.88
Build cuda_13.0.r13.0/compiler.36424714_0
```


## 4. Installing cuDNN

Vào [trang chủ chính thức](https://developer.nvidia.com/cudnn) để tải về.
`cuDNN` mặc định sẽ được cài vào `/usr/lib/x86_64-linux-gnu/` hoặc `/usr/local/cuda/lib64/`, đây là nơi mặc định linker luôn tìm nên khỏi cần add path cho `cuDNN`.



## 5. Installing Nvidia Container Toolkit 

Follow theo guide ở đây: https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html

Nhớ làm theo bước config docker nữa là xong 


----
## Một vài thứ linh tinh

## MOK management 
Vì một vài lí do mà theo gemni là do scure boot và tick chọn option install third party package thì sau khi tải nvidia driver, ubuntu bắt phải thông qua một bước nữa mới cho sử dụng đó là đặt pass MOK. Cứ đặt lại mật khẩu rồi thao tác theo trình tự sau:

1. Chọn **Enroll MOK** (đừng chọn _Continue boot_ ngay lúc này).
2. Chọn **View key 0** (để kiểm tra, nhưng có thể bỏ qua bước này).
3. Chọn **Continue**.
4. Hệ thống hỏi "Enroll the key(s)?", chọn **Yes**.
5. Nhập **mật khẩu** bạn đã tạo ở bước trên (lưu ý: bàn phím có thể không hiện ký tự `*` khi gõ, cứ gõ đúng rồi Enter).
6. Cuối cùng chọn **Reboot**.

Nếu bạn lỡ bỏ qua bước này lúc restart máy thì:
```shell
sudo update-secureboot-policy --enroll-key
```

## Tải google =))))))

```shell 
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo dpkg -i google-chrome-stable_current_amd64.deb
sudo apt --fix-broken install
```


## Tải vim và git 
```shell
sudo apt install vim 
sudo apt install git
```


## Tải bộ gõ tiếng Việt

```shell 
sudo add-apt-repository ppa:bamboo-engine/ibus-bamboo
sudo apt-get update
sudo apt-get install ibus ibus-bamboo --install-recommends
ibus restart
# Đặt ibus-bamboo làm bộ gõ mặc định
env DCONF_PROFILE=ibus dconf write /desktop/ibus/general/preload-engines "['BambooUs', 'Bamboo']" && gsettings set org.gnome.desktop.input-sources sources "[('xkb', 'us'), ('ibus', 'Bamboo')]"
```