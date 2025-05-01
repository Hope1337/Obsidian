Có nhiều cách để build, tuy nhiên mình chỉ tập trung vào hai lệnh sau:
- `pip install .`
- `pip install -e .`

Còn một số nữa như `python setup.py install` hay `python setup.py build_ext --inplace`, tuy nhiên việc chạy trực tiếp file `setup.py` đã không còn được khuyến khích dùng nữa. Và hiện tại thì mình cũng đang dùng để build package `.cpp` và `.cu` để học `CUDA` thôi cho nên dùng mấy cái đơn giản thôi nhé=]]]]]

## 1. `pip install .`

**Lệnh này sẽ tạo ra một số file và folder ở working directory hiện tại**:
- **build/**: Thư mục tạm thời chứa các file trung gian trong quá trình build (ví dụ: file object .o, file biên dịch từ `.cpp` hoặc `.cu`).
- **dist/**: Chứa các file phân phối như `.tar.gz` hoặc `.whl` (wheel) sau khi build package.
- ***.egg-info/**: Thư mục chứa metadata của package (ví dụ: your_package.egg-info/). Bao gồm các file như PKG-INFO, SOURCES.txt, dependency_links.txt, v.v.
- **File tạm thời**: Một số file biên dịch tạm thời như `.so` (shared object) hoặc `.pyd` (trên Windows) có thể xuất hiện trong thư mục nguồn hoặc thư mục build.

**Trong môi trường cài đặt (site-packages)** :
- Sau khi chạy pip install ., package sẽ được cài đặt vào thư mục Python environment (thường là <python_env>/lib/pythonX.Y/site-packages/).
- Các file/folder được cài đặt bao gồm:
    - **your_package/**: Thư mục chứa các file .py của package.
    - **File biên dịch**: Các file .so (Linux/macOS) hoặc .pyd (Windows) được biên dịch từ .cpp hoặc .cu.
    - **your_package.egg-info/** hoặc metadata tương tự trong site-packages.

==Note==: `site-packages` là thư mục chứa các gói và thư viện bên ngoài mà bạn đã cài đặt thông qua pip.

> Mấy file/folder này có tác dụng gì vậy?

- **build/**:
    - Chứa các file trung gian trong quá trình biên dịch (.o, .so, .pyd, v.v.).
    - Được sử dụng để lưu trữ tạm thời trước khi các file cuối cùng được chuyển vào site-packages.
    - Có thể xóa an toàn sau khi cài đặt xong.
- **dist/**:
    - Chứa các file phân phối (.tar.gz hoặc .whl) để chia sẻ hoặc cài đặt package trên các hệ thống khác.
    - File .whl là định dạng nén, chứa toàn bộ package và các file biên dịch, giúp cài đặt nhanh hơn.
- ***.egg-info/**:
    - Chứa metadata của package (tên, phiên bản, dependencies, v.v.).
    - Giúp Python và pip nhận diện package đã cài đặt và quản lý dependencies.
- **File trong site-packages**:
    - Các file .py: Chứa mã Python nguồn, được chạy trực tiếp hoặc import bởi các script khác.
    - Các file .so/.pyd: Là các thư viện biên dịch từ .cpp hoặc .cu, cung cấp các hàm tính toán hiệu năng cao (ví dụ: code CUDA cho GPU).
    - Metadata trong site-packages: Được dùng để quản lý package khi gỡ cài đặt hoặc kiểm tra phiên bản.

==Xóa package==:
```bash
pip uninstall your_package_name
rm -rf build/ dist/ *.egg-info/
```

## 2. `pip install -e .`

- **File/folder được tạo:**
    - Folder `<package_name>.egg-info` trong thư mục gốc của project (nơi chứa `setup.py` hoặc `pyproject.toml`).
    - File thực thi (nếu có) trong thư mục `site-packages` của môi trường Python (thường là `venv/lib/pythonX.X/site-packages/`).
    - Một liên kết symbol (symbolic link) trong site-packages trỏ tới thư mục mã nguồn của package (do chế độ `-e`).
    - Nếu có file `.cpp` hoặc `.cu`, các file object (`.o`), shared library (`.so` hoặc `.dll`), hoặc file thực thi được tạo trong thư mục build tạm thời (thường là `build/`).
- **Vị trí:**
    - `<package_name>.egg-info`: Trong thư mục gốc của project.
    - Liên kết và file thực thi: Trong `site-packages` của môi trường Python.
    - File build trung gian (`.o`, `.so`): Trong thư mục `build/` hoặc thư mục tạm khác (phụ thuộc vào công cụ build như setuptools, cmake).

> Tác dụng của mấy file trên?

- **<package_name>.egg-info**:
    - Chứa metadata của package (tên, version, dependencies, v.v.).
    - Giúp pip và các công cụ quản lý package nhận diện package đã cài.
- **Liên kết trong site-packages**:
    - Cho phép Python import package trực tiếp từ mã nguồn trong thư mục gốc (chế độ editable -e), tiện cho việc phát triển.
- **File .so/.dll (từ .cpp, .cu)**:
    - Là các thư viện được biên dịch từ mã C++/CUDA, được Python import để chạy các hàm native.
- **File build trung gian**:
    - Lưu trữ tạm thời các file object trong quá trình biên dịch, không cần thiết sau khi cài đặt.

==Note==: điểm khác nhau với lệnh `pip install .` là lệnh `pip install -e .` tạo link đến code hiện tại thay vì cài đặt thẳng nó vào `site_packages`, nhưng nó chỉ update theo code python thôi, tức là code python bạn thay đổi thì package nó sẽ thay đổi theo, còn `.cpp` và `.cu` đã được biên dịch thành object thì phải build lại nếu có thay đổi. Hơn nữa, khi bạn `pip uninstall` thì nó cũng chỉ xóa cái link trong `site_packages` thôi, còn file obj được build ra vẫn còn trong working dir của bạn nên nếu bạn đứng ở đó mà gọi ra thì vẫn dùng được.