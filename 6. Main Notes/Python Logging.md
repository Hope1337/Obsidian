```python
import logging

logging.info('...')
```

Thấy lệnh trên quen chứ? Mình gặp người ta dùng suốt cơ mà đọc tutorial mãi chả hiểu nó hoạt động kiểu mẹ gì. May sao nay gặp một [video của một youtuber](https://www.youtube.com/watch?v=9L77QExPmI0) mà nhìn vào diagram + nghe giải thích tí thì mình thông luôn, ngon=))))))

![[Pasted image 20250715223932.png]]

Nhìn vào diagram trên, `logging` là cũng giống như logger nhưng là root. Điểm đặc biệt ở đây là mọi message đều được propagate (nếu không bị lọc bởi filter của logger tại node đó) lên parent của nó, cũng như message trong một node đều được propagate qua các handler **cho dù có bị filter của các handler khác lọc hay không**.

Mình không hiểu triết lí thiết kế này lắm, tuy nhiên mình học dùng mà, nên thôi cứ thế mà follow Và cũng chính thiết kế như này làm mình bối rối rất nhiều khi xem các tutorial, nhờ diagram này mà hiểu rồi, đội ơn đại ca youtuber lạ mặt tốt bụng=)))))


==Note==:
- Tức là giờ muốn tạo nhiều logger mỗi cái độc lập thì phải tắt `propagate` đi.
- Hoặc tạo nhiều logger nhưng chỉ có root mới có handler, tức là mọi logger đều có chung một config
- Hoặc dùng một logger là root thôi, nhu cầu dùng machine learning cơ bản cũng không có gì phức tạp

## Basic Usage

Code bên dưới đây ví dụ được đủ những thứ cần thiết cho nhu cầu logging cơ bản rồi:
```python
import logging

# ====== 1. Cấu hình root logger (log ra console) ======
logging.basicConfig(
    level=logging.DEBUG,
    format="ROOT | %(asctime)s | %(levelname)s | %(message)s"
)

# ====== 2. Tạo logger con (module-level logger) ======
logger = logging.getLogger("myapp.module")

# ====== 3. Thêm FileHandler riêng, có filemode ======
file_handler = logging.FileHandler("myapp.log", mode="w")  # Ghi đè file mỗi lần
file_formatter = logging.Formatter("FILE | %(asctime)s | %(name)s | %(levelname)s | %(message)s")
file_handler.setFormatter(file_formatter)
logger.addHandler(file_handler)

# ====== 4. Bật/tắt propagate ======
# logger.propagate = False  # Tắt nếu chỉ muốn log ra file thôi

# ====== 5. Log đủ cấp độ ======
logger.debug("Debug message")
logger.info("Info message")
logger.warning("Warning message")
logger.error("Error message")
logger.critical("Critical message")

print("Done logging. Check console and myapp.log.")

```
