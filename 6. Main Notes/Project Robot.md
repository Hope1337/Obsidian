
### Log 1: Robot phiên bản đầu tiên (siêu cấp ngố)

Bên dưới đây là hành vi của con robot=))))))

![[video_20250411_061428.mp4]]

File usd của con robot này mình lấy từ đây:
[GitHub - Creador270/IsaacLabExtensionSpotMicro: A project on SpotMicro on Isaac Lab](https://github.com/Creador270/IsaacLabExtensionSpotMicro/tree/main)

Theo như mình thấy thì repo này cũng làm trên isaaclab, tuy nhiên không hiểu sao nó có hành vi rất lạ:

- Khi apply default joint pos, tức là liên tục gửi tín hiệu yêu cầu robot phải giữ nguyên tư thế ban đầu (tư thế đứng) của nó, robot không đứng được, liên tục ngã (như video trên).
- Các joint không tuân theo upper và lower limit của nó, thậm chí nó còn tự va vào nhau (đi xuyên qua nhau).

Ban đầu mình có một số quan sát sau:

1. Mình dùng actuator được cài đặt sẵn và dùng các setting mình tìm thấy trong asset khác nên có khả năng đây là nguyên nhân khiến robot không hoạt động ổn định.
2. Khi tăng giảm dt của simulation, mình thấy dt càng lớn, các model có kích thước nhỏ càng không ổn định, chỉ một tác động nhỏ vào joint cũng khiến chân robot giật liên tục. Microspot (con robot ở trên) có kích thước xấp xỉ 1/2 con spot có trong isaaclab, mình cũng thử scale down con spot và nó cũng mất ổn định như vậy.

Vấn đề ở (1), mình cho rằng khi train được network thì các hệ số tỉ lệ sẽ được model “hấp thụ” vào mạng và sẽ không còn quan trọng lắm. Ở (2) thì mình đã scale microspot lên, tuy các thông số về khối lượng tuy có khác nhau nhưng nhìn chung vẫn khá hợp lí và mình nghĩ là không có vấn đề gì.

Sau một quá trình train đi train lại nhiều lần, cộng thêm cả việc điều chỉnh rất nhiều thứ trong setting của asset như actuator thì robot vẫn không di chuyển được, nó học được một policy là nằm yên và không chuyển động.

Sau rất nhiều lần thử, mình có thêm một số quan sát sau:

- Ở các iteration train ban đầu, có một số robot chỉ di chuyển chân nhẹ nhưng bay một quãng xa lên trời, phải chăng có điều gì đó không ổn đối với vật lý của các robot này chăng?
- Cái chân phải sau luôn bị xoắn lại, không hiểu tại sao
- Khi in ra joint pos lúc apply default joint pos, joint pos liên tục dao động càng ngày càng xa khỏi default của nó

Hãy nhìn hành vi của nó xem này=)))))))

![[065b3de5-66e9-404c-83ff-9c4f20d62411.mp4]]

Tuy nhiên, khi mình import robot vào isaacsim, kiểm tra các joint limit và hành vi của nó khi mình tác động vào thì vẫn thấy nó hoàn toàn ổn, lúc này mình đã không còn manh mối nào nữa.

### Log 2: Robot phiên bản 2

Mình nghĩ file robot đầu tiên có lỗi gì đó nên mình đã tìm thêm các file khác, cũng là microspot robot. Tuy nhiên thì con này không nhiều người làm lắm, có nhiều bản in 3D nhưng không phải dành cho mục đích giả lập vì không có vật lý. Khá là may khi mình có tìm thấy một repo có con microspot này:

[GitHub - mike4192/spotMicro: Spot Micro Quadruped Project](https://github.com/mike4192/spotMicro)

Đây không phải project isaaclab, nhưng có vẻ như các file vật lý được khá đầy đủ thì phải.

Tuy nhiên có cái gì đó sai sai với mấy file này, khi mình export ra file usd theo như hướng dẫn trong [document chính thức](https://isaac-sim.github.io/IsaacLab/main/source/how-to/import_new_asset.html) của isaaclab thì nó bị mất bộ phận:

![[1aa0171e-25b8-41a6-a5b2-f8f926aa8a9d.jpg]]

I have no idea what’s going on, nhưng hình như [có người cũng bị](https://github.com/isaac-sim/IsaacLab/issues/885) như mình thì phải.

P/s: lỗi này sau này mình không gặp nữa, nhưng mình cũng không biết tại sao:v

Note: Không nên import bằng urdf rồi save lại dưới dạng usd, mình không biết tại sao (cái j cũng không biết:v) nhưng nó sẽ có một số lỗi rất lạ, ví dụ như robot sẽ bị treo lơ lửng thế này - sau này thì mình mới biết do mình để config fixed base - hoặc gia tốc trọng trường của robot lớn kinh khủng khiếp, vừa spawn ra là nó đâm đầu thẳng xuống đất (rất nhanh) luôn. Hơn nữa cái vật lý của con này nó cũng có vấn đề, hãy xem này:

![[6499636408822.mp4]]

Tại sao nó lại vậy ấy hả? Thời điểm đó mình cũng không biết.

### Log 3: Fix Inertia

Hóa ra có một thứ trong file robot đó là inertia, mình cũng không rõ tại sao lại phải specify cái này mà không để máy tính tự tính? Isaacsim có doc hướng dẫn tính inertia cho asset, tuy nhiên mình chỉ thấy họ chỉ cách tính cho mấy khối đơn giản. Cơ mà ngạc nhiên là anh Huy ốp cái inertia của khối đơn giản vào thì nó hoạt động mượt mà=))))))).

Còn một điểm nữa, robot có một body part là `toe_link`, tuy nhiên không hiểu sao nó lại bị gộp chung với `foot_link`. Mình có hỏi Grok thì nó bảo là chuyển từ `fixed_joint` sang `revolution`, cơ mà mấy con robot khác vẫn dùng `fixed_joint` bình thường đó thôi.

À với cả code của mình có bug “nhẹ” một chỗ, sau khi fix xong, kết hợp hết mọi thứ ở trên lại thì cuối cùng nó cũng chịu hoạt động rồi:>>>>>

![[out-3.mp4]]