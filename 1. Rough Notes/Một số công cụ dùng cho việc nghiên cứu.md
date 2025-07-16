
Chào mọi người, trong bài viết này mình sẽ giới thiệu một vài công cụ mà mình cảm thấy rất hữu ích cũng như bắt gặp rất nhiều trong quá trình nghiên cứu. Chà, gọi là công cụ cũng không hợp lí lắm bởi vì nó có thể là vài package trong python hay thậm chí là hệ điều hành nữa:v, mà thôi tạm gọi vậy nhé.

À mà nói trước, trong blog này mình chỉ giới thiệu các công cụ này là gì và tại sao mình recommend thôi nhé, còn dùng thế nào hay cài đặt ra sao thì các bạn nên search trên mạng, hiện có rất nhiều tutorial cho mấy tool này rồi. Tất nhiên các tool mình đề cập là free, không mất phí đâu nhé.

---
#### Ubuntu

Nếu mọi người không mò mẫm nhiều về máy tính (như mình hồi xưa), chủ yếu chỉ dùng cái gì đó cho nó tiện thì việc trở thành perennial window user là điều dễ hiểu. Cơ mà sau này khi làm nghiên cứu cũng như vọc nhiều thứ với máy tính hơn, mình cảm thấy thích dùng ubuntu và nó cũng mang lại nhiều lợi ích cho mình hơn. Dưới đây là một số lí do mà bạn nên dùng ubuntu:
- Open source, free, khỏi mất công crack (lớn rồi đừng crack nữa nhé=)))) ), hoặc tốn nhiều tiền mua bản quyền.
- Rất nhiều code của các nhóm nghiên cứu mình toàn thấy dùng ubuntu thôi. Và bạn biết gì không? Các package (mấy cái mà bạn hay dùng pip install để tải á) rất khác nhau trên window và ubuntu. Vì vậy việc các bạn reproduce lại môi trường thôi nghe đã khó rồi. Tất nhiên là có cách khác như dùng Docker (tí nữa nói về cái này sau), cơ mà theo mình thì vẫn nên dùng ubuntu nhé.
- Chạy code python nhanh hơn window (mình nghe bảo vậy, chưa thử đo bao giờ)
- Nhẹ hều, chạy siêu mượt so với window.

Tất nhiên ubuntu có kha khá nhược điểm khác như việc tải và cài đặt các phần mềm thường là lằng nhằng hơn. Hay việc có nhiều game và phần mềm không được support như Office (tất nhiên ubuntu có office riêng, mà nó dỏm lắm). Nên mình nghĩ phương án tốt nhất là dual boot, tức là cài song song window với ubuntu á, cái này các bạn lên mạng xem nhiều hướng dẫn lắm.

---

#### Conda

Tưởng tượng thế này, bạn có hai project khác nhau, tạm gọi là A và B. A yêu cầu package C với version 2.5 còn B yêu cầu C với version 3.0. Thật không may là hai version này không tương thích ngược với nhau và rõ ràng bạn không cài đặt một lúc 2 version cho C được. Đây là chuyện rất thường xảy ra, nhưng may là chúng ta hiện có rất nhiều công cụ quản lý gói, tức là tạo ra nhiều môi trường độc lập hơn.

`conda` là một trong nhiều công cụ quản lý gói hiện nay, bên cạnh `pip`, `poetry`, `virtualenv` .... Bạn có thể xài bất kì cái nào nếu thích, tuy nhiên mình recommend dùng `conda` vì nó specify cho khoa học dữ liệu.

---

#### Docker

Cái này khá giống như `conda` nhưng ở mức cao hơn. Tưởng tượng bạn có hai code mà yêu cầu về cài đặt của chúng hoàn toàn khác nhau ví dụ như hệ điều hành, package.... Làm sao chạy hai code này đây?

> Thế chẳng phải `conda` cũng cô lập các môi trường thôi sao? Cứ dùng `conda` thôi?

Đúng là `conda` có thể tạo nhiều môi trường và chỉ cần tạo hai `env` khác nhau rồi chạy code thôi. Tuy nhiên bạn đã nghĩ đến cho hai code đó giao tiếp thế nào chưa? Ví dụ chúng cần chuyển tensor cho nhau? Ghi ra file hả? Hay giả sử hai code yêu cầu khác cả hệ điều hành thì sao?

Lúc này docker xuất hiện như một giải pháp, nó giống như cung cấp cho bạn một máy ảo để code vậy, mọi thứ liên quan đến cài đặt đều được isolate vào một chỗ gọi là `container`. Nói là máy ảo cho dễ hiểu thôi chứ thực ra cách nó hoạt động khác với máy ảo đó, cái này lên mạng mà xem nghen=)))))).

==Note==: Có thể lúc đầu nghe qua docker với conda nó cứ bị chồng lấn công dụng với nhau, nhưng mình rất hay dùng hai cái này chung với nhau và mỗi thằng có một chức năng riêng. Mình nghĩ các bạn phải thực sự dùng mới hiểu được, khám phá đi.

---

#### Obsidian

Đây là công cụ viết note mã nguồn mở, mình rất hay dùng để note lại mấy thứ mình học được, cô đọng và giải thích theo cách hiểu của mình. Cái này theo mình thấy có hai công dụng:
- Note lại, phòng khi nào quên. Có những thứ sau khi học mình khư khư rằng không bao giờ quên được, vậy mà mới có vài tháng không đụng vào là bay sạch kiến thức rồi.
- Khi bạn học và bạn giải thích lại điều bạn học một lần nữa, nó giúp củng cố những gì bạn hiểu. Hơn nữa, trong lúc bạn trình bày lại theo ý của bạn, nó giúp bạn kiểm tra xem bạn có thực sự hiểu vấn đề chưa. Nhiều lúc đang note mà mình có những khoảnh khắc như "đúng ra công thức đoạn này phải là như kia, như này sai rồi", lúc đó mình double check lại và một lần nữa kiểm tra kiến thức của mình. Mình thấy cách này rất hiệu quả.

Ngoài obsidian ra còn nhiều công cụ take note khác như notion chẳng hạn, nhưng mà quan trọng là obsidian nó free=))))))).

---

#### Latex

Latex là một ngôn ngữ soạn thảo giúp bạn có được những biểu thức toán rất rất đẹp, thử nhé:
$$
\begin{align*}
G_{t:t+2} &\doteq R_{t+1} + \gamma \sum_{a \neq A_{t+1}} \pi(a \mid S_{t+1}) Q_{t+1}(S_{t+1}, a) \\
&\quad + \gamma \pi(A_{t+1} \mid S_{t+1}) \left( R_{t+2} + \gamma \sum_{a} \pi(a \mid S_{t+2}) Q_{t+1}(S_{t+2}, a) \right) \\
&= R_{t+1} + \gamma \sum_{a \neq A_{t+1}} \pi(a \mid S_{t+1}) Q_{t+1}(S_{t+1}, a) 
+ \gamma \pi(A_{t+1} \mid S_{t+1}) G_{t+1:t+2},
\end{align*}
$$
Công thức này là mình soạn bằng latex đó.

Latex được dùng rất nhiều khi viết paper, thường là họ sẽ cung cấp cho chúng ta một template mẫu, ta chỉ cần thêm nội dung vào thôi. Tài liệu được viết bằng latex rất là đẹp.

Nhắc lại latex chỉ là ngôn ngữ thôi, muốn dùng thì phải có trình soạn thảo (mình thường dùng TeXstudio) và trình biên dịch (mình thường dùng TeXlive). Tất nhiên là bạn có thể dùng overleaf (bản web), tuy nhiên nó là bản giới hạn (unlock thì trả phí) nhưng cho bạn colab với nhiều người hơn. Riêng mình thì thích offline hơn vì compile nhanh và không có bị giới hạn. Cái này tùy vào sở thích.

---

#### Zotero

Bạn đọc và lưu trữ các paper thế nào? Tạo folder rồi save vào hay mỗi lần đọc là lại search? Lúc cite thì lại phải search + copy hả? 

Zotero là một trình quản lý tài liệu, cho phép bạn lưu trữ các file, note, metadata (thông tin tác giả, citation, note ...), rất tiện để quản lí. Ngoài ra nó còn có một trình đọc pdf riêng, giúp bạn take note hoặc highlight trong lúc đọc paper nữa đó.


---

#### `logging` và `argparse`

Đây là hai package trong python mà mình khá chắc là bạn sẽ bắt gặp rất rất nhiều trong lúc đọc code.

- `logging` cho phép bạn tạo log, nếu biết cách dùng, nó tiện hơn rất nhiều việc mỗi lần cứ print ra.
- `argparse` cho phép bạn tạo các flag khi dùng CLI cho code của bạn.

Hai cái này cú pháp nhìn hơi lê thê xíu, cơ mà dùng nhiều sẽ quen và bạn sẽ cảm thấy tại sao không học dùng nó sớm hơn đó.

---

#### Vim

Vâng=)))))))

Có khi nào bạn code hoặc edit gì đó mà quá lười dùng chuột không, đây là siêu giải pháp cho bạn. Vim rất khó dùng vì nó toàn mấy cú pháp ở đâu ra không, rất khó nhớ và cũng rất là nhiều. Thời gian đầu học thậm chí nó có thể trông chậm hơn việc dùng chuột nhiều nhưng tin mình đi, thời gian trôi bạn sẽ thấy vim là chân ái. Mình bây giờ nghiện dùng vim rồi
