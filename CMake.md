Khi bạn code python, việc bạn làm chỉ là tạo một env ảo, muốn xài package ngoài? Pip install hoặc conda install, hoặc bất kì trình quản lý package nào bạn muốn, sau đó chỉ cần `python source.py` là được.

Tuy nhiên `C++` không đơn giản vậy, nó là trình biên dịch và nếu muốn cài package khác bên ngoài, bạn sẽ cần specify `.h`, `.so`, `.a` (hoặc `.dll` và `.lib` nếu trên window) cho trình biên dịch biết bạn đang dùng cái gì và cần tìm nó ở đâu. Rất lằng nhằng so với python phải không?

Một điều nữa là khi số lượng package bên ngoài cũng như một số thao tác build khác mà bạn muốn thêm vào trong quá trình build sẽ gồm rất nhiều lệnh. Và tất nhiên rồi, ta có thể dồn hết các instructions đó vào một file gọi là `cmake` file. Hay nói chính xác hơn, `cmake` giúp ta tự động generate ra `makefile` là instructions để build được code.

