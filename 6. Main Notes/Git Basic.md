> Vấn đề của chúng ta là gì?

Đây luôn là câu hỏi mở đầu khi chúng ta muốn học về một công cụ mới. Công cụ sinh ra để giải quyết một vấn đề nào đó trong cuộc sống, thế cho nên rất tự nhiên khi đặt một câu hỏi thế này để ta có một cái cái nhìn khái quát về ngữ cảnh, công dụng và ý nghĩa của công cụ ta đang tìm hiểu.

Quay trở lại câu hỏi ở trên, hồi năm nhất đại học, lúc tự phát triển một model thị giác máy tính để viết paper thì bản thân mình cũng phải trải qua một số vấn đề sau:

- **Lưu backup của code**: Mình khá là hay quên, có khi mới code lúc sáng, tối dọn dẹp máy quên mất xong xóa folder chứa file code luôn:v, hoặc chỉ đơn giản là muốn lưu lại một version mà mình khá ưng ý nhưng lưu xong khoảng một tháng sau là kí ức vị trí của nó mất tiêu không một dấu vết.

- **Đồng bộ code giữa các thiết bị**: Mình làm việc trên cả PC và laptop, mỗi lần muốn đồng bộ là phải gửi qua zalo, mà zalo không cho gửi folder nên phải gửi từng file, muốn dùng drive thì quá trình up lên drive và tải về giải nén ra thì nó lâu điên, mỗi lần như vậy rất là phiền.
  
- **Nhiều nhánh thử nghiệm khác nhau**: Nhiều lúc từ một version, mình có kha khá ý tưởng để cải tiến nhiên phải chia code base ra nhiều nhánh khác nhau, mỗi lần như vậy mình phải note lại từng thứ, rất mệt. Hơn nữa để đổi code từ ver này qua ver khác và code cũng rất tốn thời gian.

- **Hợp nhất các phiên bản**: Sau khi thực nghiệm một số thứ trên một vài nhánh dev, mình muốn hợp nhất nó lại với nhau thành phiên bản hoàn thiện, đây mới là ác mộng khi có một tỉ thứ mình đã thay đổi, và chỉ mới tính trong một file. Mình phải vác từng file trong folder code lên mấy tool compare file để check, có khi đoạn code cũ lắm rồi thì phải ngồi nhớ xem mình làm cái gì ở đấy, giờ đổi thì nó có lỗi không. Hợp nhất xong còn file test từng thứ xem có gì lỗi hay không.

Đấy là những vấn đề mà mình gặp phải trong quá trình phát triển dự án của mình, thật may, đây cũng là vấn đề xuất hiện từ rất lâu rồi và một trong những công cụ phổ biến nhất để giải quyết nó đó chính là **Git**. 

==Một chút kiến thức bên lề==: **Git** là một **V**ersion **C**ontrol **S**ystems (VCS), cụ thể hơn là **Distributed** Version Control System. Tức là mỗi máy sẽ có toàn bộ lịch sử thay đổi của code, và nếu muốn thì người ta mới push nó lên một nơi lưu trữ chung mà mọi người có thể chia sẻ (**Git hub**). Điều này trái ngược với **Centralized** VCS, nơi chỉ có một máy chủ lưu code và lịch sử code cũng chỉ lưu tại nơi đó.

==Note==: Bạn có thể đã từng nghe nói tới **Git hub**, tuy nhiên cần lưu ý là **Git** và **Git hub** là hai thứ khác nhau nha:
- **Git**: là một VCS, cũng là một công cụ chạy local trên máy của bạn.
- **Git hub**: là một nơi lưu trữ code (dịch vụ lưu trữ đám mây), có cung cấp thêm một số tính năng khác nữa (mình sẽ không đề cập ở đây vì mình cũng ko biết dùng=))), chắc là trong tương lai mình cũng sẽ cần đến thôi, nhưng đấy là trong tương lai)

Hồi cấp 3 mình cũng có nghe qua **Git** và **Git hub**, tuy nhiên tất cả những gì mình biết về nó là: Một nơi để lưu trữ code (thực ra cái này đúng với **Git hub**, còn **Git** mới có khả năng làm những điều trên) thế cho nên tới năm thứ 2 đại học mình mới bắt đầu nhận ra những thứ mà **Git** có thể làm và học cách sử dụng **Git** một cách tử tế.


