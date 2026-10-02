+++
title = "An toàn thông tin cơ bản – Bài 1: Vì sao vibe code cần an toàn thông tin"
date = 2026-09-30T09:01:00+07:00
draft = false
tags = ["bảo mật", "an toàn thông tin", "vibe coding"]
series = ["An toàn thông tin cơ bản"]
series_weight = 1
+++

Mình muốn mở series bằng một câu chuyện của người khác.

Tháng 3/2025, người sáng lập Enrichlead, một dịch vụ tìm khách hàng tiềm năng cho đội bán hàng, khoe trên mạng xã hội rằng toàn bộ sản phẩm do Cursor viết, không có dòng nào viết tay. Vài ngày sau khi ra mắt, chính người đó đăng lên: sản phẩm đang bị tấn công. Hạn mức API bị dùng cạn, người dùng vượt qua được gói trả phí, dữ liệu bị tạo lung tung.

Theo các bài phân tích về vụ này, nguyên nhân khá cơ bản. Việc kiểm tra gói trả phí chỉ nằm ở giao diện, nên đổi một giá trị trong trình duyệt là dùng được tính năng trả phí. API key nằm ngay trong JavaScript phía client, ai mở tab Network cũng thấy. Người sáng lập nhờ Cursor sửa tiếp thì theo lời kể, các chỗ khác lại hỏng theo, và sản phẩm đóng cửa.

Điều đáng chú ý là sản phẩm đó chạy ổn khi demo. Không có thông báo lỗi nào, không có gì đỏ trên màn hình. Mọi thứ hoạt động đúng theo cách người làm ra nó thử. Chỉ đến khi người khác thử những điều người làm không nghĩ tới thì mọi chuyện mới lộ ra.

Bài này nói về chuyện đó, và vì sao khi vibe code nhiều thì chuyện đó càng đáng để ý.

<!--more-->

## Bài này dành cho ai

Cho bạn nào đang viết code, hoặc đang nhờ AI viết code, và chưa học gì về bảo mật. Bạn không cần biết trước thuật ngữ nào.

Ví dụ bên dưới dùng Python và module `sqlite3` có sẵn, không cần cài thêm gì. Mình chạy thử trên Python 3.11; code không dùng gì riêng của bản mới, nên các bản 3.x gần đây đều chạy được.

## Thu nhỏ vụ Enrichlead lại

Enrichlead hơi to để ngồi mổ xẻ, nên mình thu nhỏ lại thành một hàm đăng nhập. Đây là kiểu code mà một trợ lý AI, hay một người đang vội, rất dễ viết ra:

```python
import sqlite3

conn = sqlite3.connect(":memory:")
conn.execute("CREATE TABLE users (name TEXT, password TEXT)")
conn.execute("INSERT INTO users VALUES ('an', 'mat-khau-cua-an')")


def dang_nhap(name, password):
    # Ghép thẳng dữ liệu người dùng nhập vào câu lệnh SQL
    query = f"SELECT name FROM users WHERE name = '{name}' AND password = '{password}'"
    return conn.execute(query).fetchone() is not None


print(dang_nhap("an", "mat-khau-cua-an"))
print(dang_nhap("an", "sai"))
print(dang_nhap("an", "' OR '1'='1"))
```

Trước khi chạy, bạn thử đoán xem ba dòng `print` in ra gì. Hai dòng đầu dễ: đúng mật khẩu thì `True`, sai thì `False`. Dòng thứ ba thì sao?

```text
True
False
True
```

Dòng thứ ba cũng `True`. Người này không biết mật khẩu của `an`, chỉ gõ một chuỗi có dấu nháy đơn, và hàm vẫn cho vào.

Để hiểu vì sao, hãy nhìn câu lệnh SQL thật sự được gửi đi sau khi ghép chuỗi:

```text
SELECT name FROM users WHERE name = 'an' AND password = '' OR '1'='1'
```

Phần `OR '1'='1'` luôn đúng, nên điều kiện tìm người dùng luôn thỏa. Dữ liệu người dùng gõ vào đã biến thành một phần của câu lệnh. Lỗi này tên là SQL injection, bài 5 sẽ quay lại.

Sửa thì chỉ cần đổi vài dòng: đưa dữ liệu vào qua tham số thay vì ghép chuỗi.

```python
import sqlite3

conn = sqlite3.connect(":memory:")
conn.execute("CREATE TABLE users (name TEXT, password TEXT)")
conn.execute("INSERT INTO users VALUES ('an', 'mat-khau-cua-an')")


def dang_nhap(name, password):
    # Dữ liệu người dùng nhập đi qua tham số, không nằm trong câu lệnh
    query = "SELECT name FROM users WHERE name = ? AND password = ?"
    return conn.execute(query, (name, password)).fetchone() is not None


print(dang_nhap("an", "mat-khau-cua-an"))
print(dang_nhap("an", "sai"))
print(dang_nhap("an", "' OR '1'='1"))
```

```text
True
False
False
```

Cả hai phiên bản trả lời giống hệt nhau với mọi thử nghiệm bình thường. Chỉ khi có người cố tình gõ thứ kỳ lạ thì chúng mới khác nhau. Nếu bạn là người viết ra bản đầu tiên rồi chạy thử hai trường hợp đúng và sai, bạn sẽ thấy mọi thứ ổn và đi làm việc khác. Y hệt Enrichlead.

Còn một lỗi nữa trong ví dụ mà bạn có thể đã thấy: mật khẩu được lưu nguyên văn trong cơ sở dữ liệu. Bài 4 sẽ nói cách lưu đúng.

## AI có hay viết code kiểu này không

Veracode, một công ty bảo mật ứng dụng, từng cho hơn 100 mô hình ngôn ngữ lớn làm 80 tác vụ lập trình. Mỗi tác vụ có một cách viết an toàn và một cách viết không an toàn, và họ xem mô hình chọn cách nào. Báo cáo năm 2025 ghi nhận khoảng 45% lần thử cho ra code có lỗ hổng rủi ro. Báo cáo năm 2026 gần như không đổi: khoảng 44%, tỷ lệ đạt trung bình 56%.

Đừng đọc con số này như xác suất đoạn code của bạn có lỗi. Đó là kết quả trên một bộ bài do một công ty thiết kế. Nhưng có một điểm đáng nhớ: theo báo cáo, các mô hình ngày càng viết code đúng cú pháp và chạy được, còn chuyện viết code an toàn thì không tiến bộ theo cùng tốc độ. Hai chuyện này khác nhau, và ví dụ đăng nhập ở trên là minh họa khá rõ.

## Vì sao lỗi cứ lọt qua

Mình có vài suy đoán, và đây là nhận định của mình, không phải kết luận của nghiên cứu nào.

Thứ nhất, yêu cầu thường chỉ mô tả chức năng. "Viết giúp mình trang đăng nhập" không nói gì về chuyện người dùng gõ dữ liệu có hại, nên mô hình làm đúng điều được nhờ.

Thứ hai, chúng ta test theo đường đi bình thường. Người dùng thật hiếm khi gõ `' OR '1'='1`, nên người viết cũng không nghĩ đến chuyện thử.

Thứ ba, code do AI viết mà chạy được ngay thì thói quen đọc từng dòng giảm đi. Chỗ lỗi ẩn nằm lại chính ở đó.

Và quan trọng nhất, lỗi bảo mật không kêu. Ứng dụng không dừng, không in lỗi. Nó chỉ lộ ra khi có người khai thác, như Enrichlead.

## Series sẽ đi qua những gì

Cho ai muốn biết trước, đây là 12 bài dự kiến:

- Bài 1: bài này.
- Bài 2: tư duy bảo mật, tức là nghĩ như người muốn phá ứng dụng của mình.
- Bài 3: mật mã cơ bản.
- Bài 4: xác thực và phân quyền.
- Bài 5: bảo mật ứng dụng web, gồm SQL injection ở trên.
- Bài 6: secret và cấu hình, tức là chuyện API key nằm lung tung như ở Enrichlead.
- Bài 7: dependency và chuỗi cung ứng.
- Bài 8: bảo mật khi dùng AI coding assistant.
- Bài 9: mạng, cloud và container.
- Bài 10: con người và thiết bị.
- Bài 11: dữ liệu và quyền riêng tư.
- Bài 12: giám sát, ứng phó sự cố và học tiếp.

Các nhóm lỗi trong series bám theo [OWASP Top 10:2025](https://owasp.org/Top10/2025/), danh sách rủi ro phổ biến nhất của ứng dụng web do tổ chức OWASP công bố. Bài 2 đến 4 là nền tảng nên đọc theo thứ tự. Các bài sau khá độc lập, bạn cần phần nào thì nhảy tới phần đó.

## Tóm tắt

- Code chạy đúng với đường đi bình thường chưa chắc an toàn. Lỗi bảo mật chỉ lộ ra khi có người cố tình làm điều bất thường.
- Enrichlead và hàm đăng nhập ở trên cùng một kiểu: demo chạy ổn, không có tín hiệu lỗi nào, người ngoài thử là vỡ.
- Theo Veracode, khoảng 44 đến 45% tác vụ sinh code bằng AI trong bộ thử nghiệm của họ có lỗ hổng rủi ro, và con số ít thay đổi theo thời gian.
- Một câu lệnh SQL ghép chuỗi từ dữ liệu người dùng có thể cho vào tài khoản mà không cần mật khẩu. Truyền dữ liệu qua tham số là cách sửa.

## Tài liệu tham khảo

- [Vibe Graveyard: Enrichlead](https://vibegraveyard.ai/story/enrichlead-vibe-coded-saas-shutdown/)
- [Kaspersky: Security risks of vibe coding and LLM assistants for developers](https://www.kaspersky.com/blog/vibe-coding-2025-risks/54584/)
- [Veracode GenAI Code Security Report 2026](https://www.veracode.com/blog/2026-genai-code-security-report-ai-risk/)
- [Veracode GenAI Code Security Report 2025](https://www.veracode.com/resources/analyst-reports/2025-genai-code-security-report/)
- [OWASP Top 10:2025](https://owasp.org/Top10/2025/)
- [Tài liệu Python: sqlite3, dùng placeholder cho tham số](https://docs.python.org/3/library/sqlite3.html#sqlite3-placeholders)
