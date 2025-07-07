
## Folder ảnh
- `\graphicspath{ {./images/} }`: khai báo folder để ảnh
-  `\graphicspath{ {./images1/}{./images2/} }`: hoặc cũng có thể khai báo nhiều folder để $\LaTeX$ tự tìm.

> The file name of the image should not contain white spaces nor multiple dots

---

## Các tham số khi insert ảnh

| Tham số                                | Ý nghĩa                         | Ghi chú                                                                                                                                            |
| -------------------------------------- | ------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| `width=...`                            | Đặt chiều ngang ảnh             | Có thể dùng đơn vị: `cm`, `pt`, hoặc `\textwidth`, `\linewidth`, v.v.                                                                              |
| `height=...`                           | Đặt chiều dọc ảnh               | Nếu không dùng `keepaspectratio`, ảnh có thể bị méo                                                                                                |
| `scale=...`                            | Phóng to/thu nhỏ ảnh theo tỷ lệ | `1` là giữ nguyên kích thước                                                                                                                       |
| `angle=...`                            | Xoay ảnh theo độ (degree)       | `angle=90` xoay 90 độ theo chiều kim đồng hồ                                                                                                       |
| `keepaspectratio`                      | Bảo toàn tỉ lệ khung hình       | Dùng chung với `width` hoặc `height`. Tức là giờ đây `width` và `height` tạo ra một khung, `keepaspectratio` sẽ tự độ co ảnh sao cho vừa khung đó. |
| `origin=c`                             | Gốc xoay ở **center**           | `c` là center, `t` top, `b` bottom, `l`, `r` cũng dùng được                                                                                        |
| `clip`                                 | Cắt ảnh theo bounding box       | Cần phối hợp với `trim`, tức là `clip` kích hoạt lệnh cắt thì `trim` mới có hiệu lực                                                               |
| `trim={<left> <bottom> <right> <top>}` | Cắt lề ảnh                      | Các giá trị tính bằng đơn vị đo như `cm`, `pt`...                                                                                                  |

**Một số tham số cho `width`**

| Đơn vị          | Ý nghĩa                                                                        | Khi nào dùng?                                                                      |
| --------------- | ------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------- |
| `\textwidth`    | Chiều rộng toàn khối văn bản (text body)                                       | Dùng trong tài liệu chính (article, report...)                                     |
| `\linewidth`    | Chiều rộng **của khối hiện tại** (ví dụ: trong `minipage`, `itemize`, `quote`) | Linh hoạt, dùng được trong figure nhỏ                                              |
| `\columnwidth`  | Chiều rộng 1 cột (dùng trong tài liệu 2 cột)                                   | Dùng khi bạn viết paper IEEE 2 cột                                                 |
| `\paperwidth`   | Chiều rộng toàn bộ trang giấy                                                  | Hiếm dùng, trừ khi làm poster                                                      |
| `\textheight`   | Chiều cao toàn khối văn bản                                                    | Có thể dùng để chỉnh chiều cao ảnh                                                 |
| `\baselineskip` | Khoảng cách giữa hai dòng                                                      | Thường không dùng với ảnh, nhưng đôi khi dùng để scale text hoặc spacing liên quan |

**Một số tham số cho `origin`**

|Origin|Ý nghĩa|
|---|---|
|`c`|center (tâm ảnh) ✅ thường dùng|
|`t`|top (đỉnh ảnh)|
|`b`|bottom|
|`l`|left|
|`r`|right|
|`tl`|top-left|
|`tr`|top-right|
|`bl`|bottom-left|
|`br`|bottom-right|

--- 

## Vị trí của ảnh

| Parameter | Position                                                                                                                                                                  |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| h         | Place the float _here_, i.e., _approximately_ at the same point it occurs in the source text (however, not _exactly_ at the spot)                                         |
| t         | Position at the _top_ of the page.                                                                                                                                        |
| b         | Position at the _bottom_ of the page.                                                                                                                                     |
| p         | Put on a special _page_ for floats only.                                                                                                                                  |
| !         | Override internal parameters LaTeX uses for determining "good" float positions.                                                                                           |
| H         | Places the float at precisely the location in the $\LaTeX$ code. Requires the `float` package, though may cause problems occasionally. This is somewhat equivalent to h!. |
`[!htbp]` : ép (`!`) latex chèn bảng theo thứ tự ưu tiên xuất hiện (`htbp`)


---

##  Wrapping text around figures
`wrapfigure` để wrap ảnh vào text, trông ntn:
![[InsertingImagesEx8Overleaf.png]]
Ví dụ:
```latex
\begin{wrapfigure}{r}{0.25\textwidth} %this figure will be at the right
    \centering
    \includegraphics[width=0.25\textwidth]{mesh}
\end{wrapfigure}
```
Các tham số:

| r   | R   | right side of the text                                 |
| --- | --- | ------------------------------------------------------ |
| l   | L   | left side of the text                                  |
| i   | I   | inside edge–near the binding (in a _twoside_ document) |
| o   | O   | outside edge–far from the binding                      |

--- 

## Captioning

- `\caption` nếu được đặt trên `\includegraphics` thì caption hiện ở trên, tương tự nếu đặt ở dưới. ``outercaption`` và ``innercaption`` also available.
- `\usepackage[rightcaption]{sidecap}` gói này dùng để đặt caption ở chỗ khác, có thể dùng `leftcaption`
```latex
`\begin{SCfigure}[0.5][h] 
\end{SCfigure}`
```
Sau đó thì dùng cái này để có figure với caption ở chỗ khác

---
## Reference
- `\pageref{fig:mesh1}`: in ra số trang mà fig này nằm

---

## Đơn vị

| Abbreviation | Definition                                          |
| ------------ | --------------------------------------------------- |
| pt           | A point, is the default length unit. About 0.3515mm |
| mm           | a millimetre                                        |
| cm           | a centimetre                                        |
| in           | an inch                                             |
| ex           | the height of an **x** in the current font          |
| em           | the width of an **m** in the current font           |
| \columnsep   | distance between columns                            |
| \columnwidth | width of the column                                 |
| \linewidth   | width of the line in the current environment        |
| \paperwidth  | width of the page                                   |
| \paperheight | height of the page                                  |
| \textwidth   | width of the text                                   |
| \textheight  | height of the text                                  |
| \unitlength  | units of length in the _picture_ environment.       |

---

## Multi images

- Dùng `figure` và `subfigure`
- Giãn cách theo chiều ngang giữa các phần tử là dùng `\hfill`
- Giãn cách theo chiều dọc giữa các phần tử (tức là tạo hàng mới) dùng `\vspace`
- Tham số `[b]` trong code bên dưới là **căn phần tử này sao cho đáy của nó thẳng hàng với đáy của phần tử bên cạnh**. Ngoài ra còn:
	- `[t]`: căn theo **top** (đỉnh)
	- `[c]`: căn theo **center** (giữa)
	- `[b]`: căn theo **bottom** (đáy)

```latex
\begin{figure}[htbp]
    \centering

    % Hàng 1
    \begin{subfigure}[b]{0.3\textwidth}
        \includegraphics[width=\linewidth]{image1.jpg}
        \caption{Ảnh 1}
    \end{subfigure}
    \hfill
    \begin{subfigure}[b]{0.3\textwidth}
        \includegraphics[width=\linewidth]{image2.jpg}
        \caption{Ảnh 2}
    \end{subfigure}
    \hfill
    \begin{subfigure}[b]{0.3\textwidth}
        \includegraphics[width=\linewidth]{image3.jpg}
        \caption{Ảnh 3}
    \end{subfigure}

    \vspace{1em} % Khoảng cách giữa hai hàng

    % Hàng 2
    \begin{subfigure}[b]{0.3\textwidth}
        \includegraphics[width=\linewidth]{image4.jpg}
        \caption{Ảnh 4}
    \end{subfigure}
    \hfill
    \begin{subfigure}[b]{0.3\textwidth}
        \includegraphics[width=\linewidth]{image5.jpg}
        \caption{Ảnh 5}
    \end{subfigure}
    \hfill
    \begin{subfigure}[b]{0.3\textwidth}
        \includegraphics[width=\linewidth]{image6.jpg}
        \caption{Ảnh 6}
    \end{subfigure}

    \caption{Hình gồm 2 hàng 3 cột}
    \label{fig:2x3images}
\end{figure}
```