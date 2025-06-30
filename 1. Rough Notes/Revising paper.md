## Editor:
- (1) Please address the reviewers’ comments, especially reviewer 1 and 2.
- (2) Please address the concerns on references and comparison with state-of-the-art methods.

## Reviewer 1:

- **(1) Comparative Analysis: The added comparative analysis is a strong improvement. Consider emphasizing how YOWOv3 specifically outperforms Transformer-based models in terms of both accuracy and efficiency.**
==-> OK==

- **(2) Dataset Generalizability: While the datasets used are solid choices, highlighting any plans to test YOWOv3 on additional datasets in future work could bolster its perceived generalizability.**

- **(3) Real-World Applications: It would be impactful to briefly mention potential real-world applications YOWOv3 could target, even if formal tests haven't been conducted yet.**

==Bổ sung phần Future Development and Application==
## Reviewer 2:

- **(1) Insufficient Novelty: The method of the STAD still does not fully reflect the innovation. The authors should provide a more detailed comparison with state-of-the-art methods to clearly highlight YOWOv3’s unique contributions. For example, do these modules genuinely outperform existing STAD approaches in terms of design philosophy or performance gains? The current version of the paper fails to convincingly address this question.**

Cũng giống như họ nhà YOLO và các mô hình còn lại trong bài toán object detection, Spatial Temporal Action Detection (STAD) cũng có hai hướng tiếp cận là one-stage và two-stage.
- Đối với hướng two-stage, các mô hình như VideoMAE, MViTv2 hay Slow-fast network trước tiên sẽ sử dụng các phương pháp region proposal để trích xuất ra được đối tượng, sau đó mới đưa vào classification. Cách làm tuy có độ chính xác cao nhưng bản thân model cũng đã rất nặng và cộng thêm việc chia làm hai stage phát hiện làm cách tiếp cận này không phù hợp cho các tình huống triển khai thực tế.
- Đối với hướng one-stage, họ mô hình YOWO được truyền cảm hứng từ YOLO, kết hợp cả bước detection và classification vào vào một. Dĩ nhiên độ chính xác sẽ không bằng two-stage nhưng tốc độ và tài nguyên tính toán đã được cải thiện rất đáng kể. 

Khởi điểm từ YOWOv1, model và kiến trúc đã được tinh giảm đi đáng kể cùng với hiệu năng chấp nhận được. Phiên bản tiếp theo đó, YOWOv2 nâng cấp bằng cách sử dụng các kiến trúc phức tạp hơn, các thành phần trong mạng nặng hơn, làm tăng yêu cầu về tài nguyên tính toán. Với xu hướng như vậy, chúng tôi tin rằng mô hình one stage YOWO sẽ đi vào lối mòn về nhược điểm tài nguyên tính toán.

Với mục tiêu tạo ra mô hình nhẹ nhưng hiệu quả cho bài toán STAD, chúng tôi kế thừa ý tưởng one stage detection nhưng thay vì chọn triết lý thiết kế nâng cấp mô hình theo kiểu nâng param hay GLOPs, chúng tôi tập trung vào khía cạnh không làm tăng tài nguyên tính toán khi triển khai nhưng cải thiện được hiệu suất của mô hình: Cơ chế Label assignment và Loss function đi kèm. Cùng với nhau, hai cơ chế này bổ trợ cho việc huấn luyện của mô hình, giúp mô hình của chúng tôi dù nhẹ hơn nhưng vẫn đảm bảo được hiệu suất so với mô hình tiền nhiệm là YOWOv1 và YOWOv2. Trong phần tiếp theo, chúng tôi sẽ trình bày về kiến trúc và cơ chế label assignment và loss function đi kèm.

==Purpose: highlight cơ chế Label Assignment và Loss Function đi kèm==:
-> Các công trình nghiên cứu trước tập trung vào nâng cấp kiến trúc mạng cũng như các model tiên tiến hơn.
-> VideoMAE và Slowfast sử dụng phương pháp region proposal, sau đó mới classification, điều này đi ngược lại với triết lý phát hiện một giai đoạn.
-> YOWO dùng ngưỡng IOU như bình thường để tính label assignment. YOWOv2 dùng simOTA một cách đơn giản. 
-> Nghiên cứu của chúng tôi trong khi vẫn giữ được tính đơn giản của kiến trúc mạng cũng như sự gọn nhẹ của các model được sử dụng, tập trung vào phần Label Assignment và Loss Function đi kèm mà chúng tôi cho là ảnh hưởng rất nhiều đến performance của mô hình.
-> Bổ sung các thực nghiệm chứng minh điều này


- **(2) Limited Ablation Study: More comprehensive ablations are needed to justify the design choices of each module, especially GSTD and STCA. For example, what is the performance impact of removing GSTD entirely? How does STCA compare to other attention mechanisms? However, the current paper lacks this analysis, leaving readers unable to assess the true value of the proposed components.**

-> Viết lại một tí phần ablation cho STCA.
-> Phần GSTD không nhiều nội dung để viết, chỉ nêu số
-> Có thể xem phần ablation study về việc lựa chọn backbone3D và backbone2D nằm trong wide range experiment.


- **(3) Lack of citations: Too few citations throughout this paper. The number and quality of citations fall short of the expectations for an academic paper.**

-> Do giới hạn chỉ được bao gồm tối đa 20 references: https://www.computer.org/csdl/magazine/ex/write-for-us/14365?title=Author%20Information&periodical=IEEE%20Intelligent%20Systems

## Reviewer 3

-> Recommended paper is not relevant


cmt 1.1:

cmt 1.2: 
Chúng tôi đã bổ sung section FUTURE WORK AND PRACTICAL APPLICATIONS để bổ sung thêm các thông tin được reviewer đề xuất:

cmt 1.3:
Cũng ở phần FUTURE WORK AND PRACTICAL APPLICATIONS, chúng tôi đã bổ sung thông tin thêm về các ví dụ mà trong đó YOWOv3 có thể được ứng dụng vào thực tế:

cmt 2.1:
Chúng tôi đã lược bỏ section Related Work và thay bằng RELATED WORK AND DESIGN PHILOSOPHY nhằm vừa cung cấp ngữ cảnh về các phương pháp cho STAD hiện nay, vừa giải thích sự khác biệt về triết lý thiết kế mạng của chúng tôi so với các nghiên cứu trước đó về bài toán STAD. Chúng tôi mong rằng bằng cách bổ sung thêm các thông tin trên, sự khác biệt của chúng tôi được highlight và gây được ấn tượng cũng như giúp người đọc hiểu được đóng góp của nghiên cứu của chúng tôi:

cmt 2.2:
Chúng tôi rất trân trọng góp ý của reviewer, tuy nhiên do giới hạn về số lượng kí tự, chúng tôi xin được phép chừa không gian cho các nội dung khác cũng quan trọng không kém. Mặc dù chúng tôi có khá nhiều ablation study cho phần label assignmet, affect của hàm loss trên các data có đặc tính khác nhau và nhiều tham số khác của quá trình label assignment cũng như cho module GSTD, chúng tôi rất lấy làm tiếc khi không còn chỗ để bao gồm chúng vào. Rất mong reviewer hiểu và thông cảm cho quyết định này của chúng tôi. Các setting cho các thực nghiệm nêu trên đều được chúng tôi hỗ trợ trong implementation của mình để các nhà nghiên cứu có thể thử nghiệm và tìm ra được thiết lập phù hợp cho bài toán riêng của họ.

Ngoài ra, chúng tôi đã có một chỉnh sửa nhẹ đề cập đến quan sát của chúng tôi khi thử nghiệm với GSTD module: 

cmt 2.3:
While we agree that the point raised is important, due to current limitations về giới hạn số lượng references mà một article, chi tiết này được đề cập ở đây: [link]. 
Cụ thể là chúng tôi chỉ được phép bao gồm không quá 20 tài liệu liên quan, chúng tôi rất lấy làm tiếc vì giới hạn này và mong reviewer thông cảm cho chúng tôi về việc này.


cmt 3.1:
Chúng tôi rất lấy làm tiếc do giới hạn về số lượng references được nêu ở đây [link], cũng như paper được recommend không relevent đến bài toán và topic mà chúng tôi nghiên cứu. Chúng tôi quyết định không bổ sung recommend paper vào phần references của mình, mong reviewer hiểu và thông cảm cho chúng tôi.