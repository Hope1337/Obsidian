Trong lúc vọc máy tính, chắc hẳn các bạn đã không ít lần thấy các files có đuôi như:
- `.exe`, `.lib`. `.dll` trên window
- `.so`, `.a` trên linux
Hay các thư mục như `bin`, `lib` và `include`. Vậy chúng là gì vậy?

Trước tiên, ta cần hiểu về quá trình build một code C++ thành một executable file.

1. Preprocessing, xử lý các `#include` và `#define`. 
`soure.cpp` -> `source.i`
==Note==: `#include` thực chất là copy paste code vào, cho nên để tránh include một object quá nhiều lần (có thể gây ra lỗi multi definition hoặc nặng source) thì dùng `#ifndef` hoặc `#pragma once`