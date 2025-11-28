Khi bạn dùng một package bên thứ 3, bạn chỉ cần `pip install package_A` và sau đó import vào code và gọi ra là được đúng không? Rất tiện, vậy nếu bạn có thể build code của bạn và dùng nó như một package thì quá tốt rồi.

Mình nhận ra và hiểu điều này khi làm việc với `isaaclab`, và hỡi ôi, repo của họ lồng folder dài dòng kinh khủng, thế là mình biết được thêm một repo khác code riêng nhưng lại build thành một package và dùng được chỉ bằng cách import nó vào. Làm như vậy giúp code độc lập với code của `isaaclab`, không bị các update ảnh hưởng, hơn nữa working tree cũng rất gọn. Nhờ vậy mình mới hiểu được build from source là như thế nào=))))).

template `setup.py`
```python
from setuptools import setup, find_packages

setup(
    name="ten_package_cua_ban",  # Tên dùng để pip install
    version="0.1.0",
    description="Mô tả ngắn gọn về dự án",
    author="Ten Tac Gia",
    author_email="email@example.com",
    
    # --- TỰ ĐỘNG TÌM PACKAGE ---
    # Nó sẽ quét thư mục hiện tại, tìm folder nào có __init__.py thì đóng gói
    # (Dùng cho trường hợp Flat Layout như sơ đồ trên)
    packages=find_packages(), 
    
    # --- THƯ VIỆN CẦN THIẾT ---
    install_requires=[
        "numpy",
        "requests>=2.25.0",
        # Điền các thư viện bắt buộc vào đây
    ],
    
    # --- THƯ VIỆN TÙY CHỌN (Dev/Test) ---
    # Cài bằng lệnh: pip install -e .[dev]
    extras_require={
        "dev": ["black", "flake8", "pytest"],
        "docs": ["sphinx"],
    },
    
    python_requires=">=3.11", # Phiên bản Python tối thiểu
)
```