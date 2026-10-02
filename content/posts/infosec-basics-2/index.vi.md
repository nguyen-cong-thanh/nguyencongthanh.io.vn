+++
title = "An toàn thông tin cơ bản – Bài 2: Tư duy bảo mật"
date = 2026-09-30T09:02:00+07:00
draft = false
tags = ["bảo mật", "an toàn thông tin", "vibe coding"]
series = ["An toàn thông tin cơ bản"]
series_weight = 2
+++

Nhà bạn có một cửa chính chắc chắn, khóa ba chốt. Nhưng cửa sổ phòng tắm ở tầng hai không bao giờ đóng, và chìa khóa dự phòng nằm dưới chậu cây ngay cạnh cửa. Bạn không hề cẩu thả: bạn chỉ quen nhìn nhà mình từ bên trong, nơi cửa sổ phòng tắm trông vô hại.

Người muốn vào nhà thì nhìn từ bên ngoài. Họ không quan tâm cửa chính chắc cỡ nào, họ đi vòng quanh nhà tìm chỗ yếu nhất. An toàn thông tin phần lớn là tập làm quen với việc đứng ở bên ngoài nhìn vào ứng dụng của chính mình.

Ở bài trước, Enrichlead và hàm đăng nhập đều có chỗ hở mà người làm ra không thấy, vì họ chỉ nhìn từ bên trong. Bài này là cách nhìn từ bên ngoài, đi qua vài khái niệm nền tảng, và mình sẽ dùng một ứng dụng ghi chú nhỏ xuyên suốt để các khái niệm có chỗ bám.

<!--more-->

## Bài này dành cho ai

Cho bạn nào đã đọc [bài 1](../infosec-basics-1/) hoặc đã viết được một ứng dụng web đơn giản. Bài không cần cài gì ngoài Python; đoạn code duy nhất dùng `sqlite3` và `secrets` có sẵn trong thư viện chuẩn. Mình chạy thử trên Python 3.14.

## Một ứng dụng ghi chú, và những cánh cửa của nó

Giả sử bạn nhờ AI viết một ứng dụng ghi chú. Nó có các tính năng này:

- đăng ký và đăng nhập;
- tạo, sửa, xóa ghi chú;
- đính kèm ảnh vào ghi chú;
- bấm "chia sẻ" để lấy một đường link cho người khác đọc;
- một API để ứng dụng điện thoại gọi vào;
- trang báo cáo cho bạn xem số ghi chú mỗi ngày.

Bây giờ thử đóng vai người ngoài và hỏi: ứng dụng này cho mình chạm vào chỗ nào? Form đăng nhập nhận tên và mật khẩu. Ô ghi chú nhận chữ tùy ý. Chỗ tải ảnh nhận file tùy ý. Link chia sẻ nhận một cái ID trên URL. API nhận JSON. Mỗi chỗ như vậy là một nơi dữ liệu từ bên ngoài đi vào ứng dụng, và là một nơi có thể có người gõ thứ bạn không ngờ tới.

Tập hợp những chỗ đó có tên là bề mặt tấn công. Tính năng càng nhiều, bề mặt càng rộng. Đây cũng là lý do khi vibe code, bề mặt hay rộng ra nhanh hơn người viết nhận ra: nhờ AI thêm một tính năng chỉ tốn một câu, mà mỗi tính năng lại mở thêm một cánh cửa.

Một bài tập rất rẻ: lấy giấy ra, liệt kê mọi chỗ ứng dụng của bạn nhận dữ liệu từ bên ngoài. Bạn sẽ thường thấy danh sách dài hơn mình tưởng.

## Họ muốn gì ở ứng dụng này

Có cánh cửa rồi, câu hỏi tiếp theo là người ta muốn lấy gì, làm gì khi vào được. Với ứng dụng ghi chú, thử nghĩ vài kịch bản:

Một người đọc được ghi chú của người khác. Một người sửa nội dung ghi chú của người khác mà chủ ghi chú không hay biết. Một người xóa sạch ghi chú, hoặc làm ứng dụng treo để không ai dùng được.

Ba kịch bản này tương ứng ba thứ mà giới bảo mật gọi tắt là CIA: bí mật (người không được phép thì không đọc được), toàn vẹn (dữ liệu không bị sửa khi chưa được phép), sẵn sàng (người được phép dùng được khi cần). Không phải ứng dụng nào cũng coi ba thứ này nặng như nhau. Với một ứng dụng ghi chú, bí mật có lẽ quan trọng nhất. Với một hệ thống đặt vé, sẵn sàng có thể quan trọng hơn. Việc đầu tiên là biết ứng dụng của mình sợ mất cái nào nhất.

Cái đáng bảo vệ gọi là tài sản: ở đây là nội dung ghi chú, tài khoản người dùng, và cả khả năng ứng dụng chạy bình thường. Người hoặc thứ có thể gây hại là mối đe dọa. Chỗ yếu trong ứng dụng cho phép mối đe dọa đó thành công là lỗ hổng. Còn rủi ro là cả bức tranh ghép lại: lỗ hổng đó dễ bị khai thác tới đâu, và nếu bị khai thác thì thiệt hại lớn cỡ nào.

Phân biệt lỗ hổng và rủi ro hữu ích hơn vẻ ngoài. Một trang quản trị còn bật chế độ debug là một lỗ hổng. Nếu trang đó chỉ truy cập được từ mạng nội bộ kín và không chứa dữ liệu nhạy cảm, rủi ro thấp. Nếu nó nằm công khai trên Internet và lộ cả mật khẩu cơ sở dữ liệu, rủi ro rất cao. Cùng một loại lỗi, mức cần lo khác hẳn. Biết điều này giúp bạn khỏi hoảng trước một danh sách dài các cảnh báo, và biết sửa cái nào trước.

## Bốn câu hỏi trước khi viết tính năng

Thay vì chờ đến khi có sự cố, người ta có một thói quen gọi là threat modeling: ngồi nghĩ trước xem tính năng sẽ bị làm sao. Nghe nặng nề, nhưng bản nhẹ chỉ là bốn câu hỏi. Mình thử với tính năng chia sẻ link của ứng dụng ghi chú.

Câu 1: Mình đang làm gì? Người dùng bấm "chia sẻ", ứng dụng tạo link dạng `/share/12`, ai có link thì đọc được ghi chú đó.

Câu 2: Có thể sai ở đâu? Hãy nhìn con số 12. Nếu ID chạy tuần tự, người ngoài chỉ cần thử `/share/1`, `/share/2`, `/share/3`... là đọc được mọi ghi chú từng được chia sẻ, kể cả những cái chủ nhân tưởng chỉ gửi cho một người. Ngoài ra, link đã gửi đi rồi thì không có cách thu hồi. Và nếu trang chia sẻ hiển thị nội dung ghi chú nguyên văn, một ghi chú chứa đoạn HTML có thể chạy script trên trình duyệt của người xem.

Câu 3: Mình sẽ làm gì với chúng? ID dùng một chuỗi ngẫu nhiên đủ dài thay vì số tuần tự. Thêm nút thu hồi link, và có thể cho link hết hạn. Escape nội dung trước khi hiển thị.

Câu 4: Vậy đã đủ chưa? Nhìn lại danh sách: chuỗi ngẫu nhiên giải quyết chuyện đoán ID nhưng không giải quyết chuyện người nhận chuyển tiếp link cho người khác. Cái đó phải chấp nhận hoặc có thêm lựa chọn "chỉ người đăng nhập mới xem". Ghi lại những gì chưa làm cũng là một kết quả.

Chuyện tạo ID ngẫu nhiên đủ dài chỉ cần vài dòng với thư viện chuẩn:

```python
import secrets

link_id = secrets.token_urlsafe(16)
print(len(link_id))
print(2 ** (8 * 16))
```

```text
22
340282366920938463463374607431768211456
```

Dòng đầu cho thấy ID dài 22 ký tự. Dòng sau là số lượng giá trị có thể có, khoảng 3,4 nhân 10 mũ 38. Đoán mò từng cái là không khả thi. Điểm cần nhớ là dùng `secrets` chứ không phải `random` cho những thứ như thế này, vì `random` không được thiết kế để chống lại người cố đoán.

Cả bốn câu hỏi mất khoảng mười phút nghĩ cho một tính năng, và không cần biết gì về bảo mật nâng cao. Nếu bạn vibe code, bạn có thể hỏi chính AI bốn câu này trên đoạn code nó vừa viết. Chỉ cần nhớ rằng câu trả lời đáng tin hơn khi bạn tự đọc lại và tự đối chiếu.

## Đừng dựa vào một lớp bảo vệ duy nhất

Quay lại căn nhà: cửa chính chắc không đủ, vì chỉ cần một cửa sổ hở là xong. Nên người ta chồng nhiều lớp: khóa cửa, khóa cửa sổ, đèn cảm biến, camera, hàng xóm biết mặt bạn. Lớp nào hỏng thì còn lớp khác. Trong an toàn thông tin, cách nghĩ này gọi là defense in depth.

Với ứng dụng ghi chú, "nhiều lớp" nghĩa là: kiểm tra quyền ở phía server chứ không chỉ ẩn nút trên giao diện, tham số hóa câu lệnh SQL, escape khi hiển thị, giới hạn tần suất đăng nhập, ghi log để phát hiện bất thường. Một lớp bị vượt qua không kéo theo toàn bộ sụp đổ.

Nguyên tắc đi đôi là least privilege: mỗi thành phần chỉ nên có quyền đúng bằng việc nó cần. Trang báo cáo của ứng dụng ghi chú chỉ cần đọc, vậy cho nó kết nối chỉ đọc. Nếu một ngày nó bị lợi dụng, thiệt hại dừng ở mức đọc dữ liệu chứ không xóa được:

```python
import os
import sqlite3

path = "ghi_chu.db"

# Kết nối đầy đủ quyền: dùng cho các chức năng cần ghi
conn = sqlite3.connect(path)
conn.execute("CREATE TABLE notes (id INTEGER PRIMARY KEY, body TEXT)")
conn.execute("INSERT INTO notes (body) VALUES ('mua sữa'), ('họp lúc 9h')")
conn.commit()
conn.close()

# Chức năng báo cáo chỉ cần đọc, nên chỉ cấp quyền đọc
bao_cao = sqlite3.connect(f"file:{path}?mode=ro", uri=True)
print(bao_cao.execute("SELECT COUNT(*) FROM notes").fetchone()[0])

try:
    bao_cao.execute("DELETE FROM notes")
except sqlite3.OperationalError as loi:
    print("Lỗi:", loi)

bao_cao.close()
os.remove(path)
```

```text
2
Lỗi: attempt to write a readonly database
```

Đây là SQLite trong bộ nhớ nhỏ, nhưng ý tưởng giống hệt với cơ sở dữ liệu thật: tạo một tài khoản chỉ có quyền `SELECT` cho trang báo cáo, thay vì dùng chung tài khoản có toàn quyền cho mọi thứ. Khi nhờ AI viết code kết nối cơ sở dữ liệu, nó thường dùng một tài khoản duy nhất cho tiện. Đáng để bạn xem lại chỗ đó.

## Tóm tắt

- An toàn thông tin bắt đầu từ việc nhìn ứng dụng của mình từ bên ngoài: người ngoài chạm được vào những đâu, họ muốn gì.
- Bề mặt tấn công là tất cả những chỗ dữ liệu từ bên ngoài đi vào ứng dụng. Mỗi tính năng thêm vào làm nó rộng hơn.
- Bí mật, toàn vẹn, sẵn sàng là ba thứ cần giữ. Mỗi ứng dụng sợ mất cái nào nhất thì ưu tiên cái đó.
- Lỗ hổng chưa phải rủi ro. Rủi ro phụ thuộc vào việc lỗ hổng dễ bị khai thác tới đâu và thiệt hại lớn cỡ nào.
- Bốn câu hỏi: đang làm gì, có thể sai ở đâu, sẽ làm gì với nó, đã đủ chưa. Dùng được cho từng tính năng nhỏ.
- Chồng nhiều lớp bảo vệ, và cho mỗi thành phần đúng quyền nó cần.

## Tài liệu tham khảo

- [OWASP Threat Modeling Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Threat_Modeling_Cheat_Sheet.html)
- [OWASP Top 10:2025, A06 Insecure Design](https://owasp.org/Top10/2025/)
- [NIST Cybersecurity Framework 2.0](https://www.nist.gov/publications/nist-cybersecurity-framework-csf-20)
- [CISA Secure by Design](https://www.cisa.gov/resources-tools/resources/secure-by-design)
- [Tài liệu Python: module secrets](https://docs.python.org/3/library/secrets.html)
- [Tài liệu SQLite: URI filenames, tham số mode=ro](https://www.sqlite.org/uri.html)
