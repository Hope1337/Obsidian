- Để cross-ref trở thành hyperlink thì import `hyperref` vào. Lưu ý là trong đa số trường hợp cần import `hyperref` cuối cùng. Ví dụ sử dụng:
```latex
\hypersetup{
    colorlinks=true,
    linkcolor=blue,
    filecolor=magenta,      
    urlcolor=cyan,
    pdftitle={Overleaf Example},
    pdfpagemode=FullScreen,
}
```

| Tham số                       | Ý nghĩa                                                                                            | Giá trị cụ thể                      |
| ----------------------------- | -------------------------------------------------------------------------------------------------- | ----------------------------------- |
| `colorlinks=true`             | Hiển thị các liên kết dưới dạng màu thay vì khung hộp (box). Nếu `false`, liên kết sẽ có viền màu. | `true` → dùng màu chữ thay vì khung |
| `linkcolor=blue`              | Màu của các liên kết nội bộ (ví dụ: mục lục, mục tham chiếu, các `\ref`, `\cite`)                  | `blue` → màu xanh dương             |
| `filecolor=magenta`           | Màu của liên kết tới file (dạng `\href{run:file.pdf}`)                                             | `magenta` → màu tím                 |
| `urlcolor=cyan`               | Màu của liên kết URL (dạng `\url{...}` hoặc `\href{http://...}`)                                   | `cyan` → màu xanh da trời           |
| `pdftitle={Overleaf Example}` | Tiêu đề sẽ hiển thị trong metadata của file PDF                                                    | "Overleaf Example"                  |
| `pdfpagemode=FullScreen`      | Khi mở file PDF, sẽ hiển thị ở chế độ toàn màn hình                                                | `FullScreen`                        |
```latex
\href{http://www.overleaf.com}{Something Linky} 
\url{http://www.overleaf.com}

% dùng cho self-ref
`\hypertarget{thesentence}{this sentence}`
```
