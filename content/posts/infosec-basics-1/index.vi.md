+++
title = "An toàn thông tin cơ bản – Bài 1: Vì sao vibe code cần an toàn thông tin"
date = 2026-09-30T09:01:00+07:00
draft = false
tags = ["bảo mật", "an toàn thông tin", "vibe coding"]
series = ["An toàn thông tin cơ bản"]
series_weight = 1
+++

Tháng 3/2025, người sáng lập Enrichlead, một dịch vụ tìm khách hàng tiềm năng cho đội bán hàng, chia sẻ trên mạng xã hội rằng toàn bộ sản phẩm do Cursor viết, không có dòng nào viết tay. Vài ngày sau khi ra mắt, chính người đó đăng rằng sản phẩm đang bị tấn công: hạn mức API bị dùng cạn, người dùng vượt qua được gói trả phí, dữ liệu bị tạo lung tung.

Theo các bài phân tích về vụ này, nguyên nhân khá cơ bản. Việc kiểm tra gói trả phí chỉ diễn ra ở giao diện, nên đổi một giá trị trong trình duyệt là dùng được tính năng trả phí. API key nằm ngay trong JavaScript phía client, ai mở tab Network cũng thấy. Người sáng lập nhờ Cursor sửa tiếp thì theo lời kể, các chỗ khác lại hỏng, và sản phẩm đóng cửa.

Sản phẩm đó chạy được khi demo. Không có thông báo lỗi nào cho thấy nó thiếu an toàn, vì mọi thứ hoạt động đúng với cách người làm ra nó thử. Chỉ khi người khác thử những điều người làm không nghĩ tới, vấn đề mới lộ ra.

Series này đi qua các mảng chính của an toàn thông tin, từ tư duy nền tảng đến việc dùng AI để viết code. Bài đầu tiên nói về lý do bạn nên bắt đầu.

<!--more-->

## Bài này dành cho ai

Bài dành cho người đã viết code, hoặc đang nhờ AI viết code, và chưa học gì về bảo mật. Bạn không cần biết trước thuật ngữ nào.

Đoạn code trong bài dùng Python và module `sqlite3` có sẵn trong thư viện chuẩn, không cần cài thêm gì. Mình đã chạy thử trên Python 3.11; code không dùng tính năng riêng của bản Python mới hơn, nên các bản 3.x gần đây đều chạy được.

## Code chạy được không có nghĩa là an toàn

Đây là kiểu code mà một trợ lý AI, hay một người đang vội, rất dễ viết ra cho chức năng đăng nhập:

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

Chạy file này, bạn nhận được:

```text
True
False
True
```

Hai dòng đầu là những gì bạn sẽ tự thử: đúng mật khẩu thì `True`, sai thì `False`. Dòng thứ ba là thứ kẻ tấn công thử. Người đó không biết mật khẩu của `an`, chỉ nhập một chuỗi có dấu nháy đơn, và hàm vẫn trả về `True`.

Lý do nằm ở câu lệnh SQL sau khi ghép chuỗi:

```text
SELECT name FROM users WHERE name = 'an' AND password = '' OR '1'='1'
```

Phần `OR '1'='1'` luôn đúng, nên điều kiện tìm người dùng luôn thỏa. Dữ liệu nhập vào đã trở thành một phần của câu lệnh. Lỗi này gọi là SQL injection, và bài 5 sẽ nói kỹ hơn.

Cách sửa chỉ cần đổi vài dòng: đưa dữ liệu vào qua tham số thay vì ghép chuỗi.

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

Hai phiên bản hoạt động giống nhau với mọi thử nghiệm thông thường. Chỉ khi bạn cố tình nhập dữ liệu có hại, hai bản mới khác nhau. Đó là điều khó nhất của bảo mật: lỗi không làm ứng dụng hỏng, nên bạn không có tín hiệu nào để phát hiện.

Ví dụ trên còn một lỗi nữa: mật khẩu được lưu nguyên văn trong cơ sở dữ liệu. Bài 4 sẽ nói cách lưu đúng.

## Số liệu về code do AI sinh ra

Veracode, một công ty bảo mật ứng dụng, đã cho hơn 100 mô hình ngôn ngữ lớn làm 80 tác vụ lập trình được thiết kế sao cho mỗi tác vụ có một cách viết an toàn và một cách viết không an toàn. Báo cáo năm 2025 ghi nhận code có lỗ hổng bảo mật rủi ro ở khoảng 45% số lần thử. Báo cáo năm 2026 cho kết quả gần như không đổi: khoảng 44% tác vụ có lỗ hổng, tỷ lệ đạt trung bình 56%.

Vài điểm để đọc con số này cho đúng:

- Đây là kết quả trên một bộ tác vụ do một công ty thiết kế. Nó cho biết xu hướng, không phải xác suất mà đoạn code của bạn có lỗi.
- Các mô hình viết code đúng cú pháp và chạy được ngày càng tốt, nhưng theo báo cáo, khả năng viết code an toàn không tăng theo cùng tốc độ.
- Theo báo cáo, các lỗi như XSS và log injection là những chỗ mô hình thất bại nhiều nhất. Cả hai sẽ xuất hiện lại ở phần sau của series.

Bạn không cần tin tuyệt đối vào từng con số. Chỉ cần nhớ rằng "AI viết, chạy được" chưa đủ làm bằng chứng cho "an toàn".

## Vì sao lỗi dễ lọt qua

Có vài lý do khiến vibe code dễ để lại lỗ hổng. Các lý do này là nhận định của mình, chưa phải kết luận của một nghiên cứu cụ thể.

- **Yêu cầu chỉ mô tả chức năng.** "Viết trang đăng nhập" không nói gì về việc chống người dùng nhập dữ liệu có hại. Mô hình làm điều bạn yêu cầu.
- **Kiểm tra chỉ theo đường đi bình thường.** Người dùng thật hiếm khi nhập `' OR '1'='1`, nên bạn cũng không nghĩ đến việc thử.
- **Ít đọc lại code.** Khi code do AI viết và chạy được ngay, thói quen đọc từng dòng giảm đi. Chính lúc đó lỗi ẩn dễ nằm lại.
- **Lỗi bảo mật không báo lỗi.** Ứng dụng không dừng, không in ra gì. Vấn đề chỉ lộ ra khi có người khai thác.

## Những nhóm lỗi sẽ gặp lại

Bảng OWASP Top 10 là danh sách các nhóm rủi ro phổ biến nhất của ứng dụng web do tổ chức OWASP công bố. Bản mới nhất là bản 2025, với thứ tự như sau:

| Mã | Nhóm |
|----|------|
| A01 | Broken Access Control |
| A02 | Security Misconfiguration |
| A03 | Software Supply Chain Failures |
| A04 | Cryptographic Failures |
| A05 | Injection |
| A06 | Insecure Design |
| A07 | Authentication Failures |
| A08 | Software or Data Integrity Failures |
| A09 | Security Logging and Alerting Failures |
| A10 | Mishandling of Exceptional Conditions |

Ví dụ SQL injection ở trên thuộc nhóm Injection (A05). Series không đi lần lượt theo bảng này mà chia theo cách một người làm phần mềm gặp chúng trong công việc.

## Bản đồ series

| Bài | Nội dung |
|-----|----------|
| 1 | Vì sao vibe code cần an toàn thông tin (bài này) |
| 2 | Tư duy bảo mật: mục tiêu, mối đe dọa, bề mặt tấn công |
| 3 | Mật mã cơ bản |
| 4 | Xác thực và phân quyền |
| 5 | Bảo mật ứng dụng web |
| 6 | Secret và cấu hình |
| 7 | Dependency và chuỗi cung ứng |
| 8 | Bảo mật khi dùng AI coding assistant |
| 9 | Mạng, cloud và container |
| 10 | Con người và thiết bị |
| 11 | Dữ liệu và quyền riêng tư |
| 12 | Giám sát, ứng phó sự cố và học tiếp |

Bài 2 đến 4 là nền tảng nên đọc theo thứ tự. Các bài từ 5 trở đi khá độc lập, bạn có thể nhảy đến phần cần dùng.

## Tóm tắt

- Code chạy đúng với đường đi bình thường chưa chắc an toàn. Lỗi bảo mật thường chỉ lộ ra khi có người cố tình nhập dữ liệu có hại.
- Theo báo cáo của Veracode, khoảng 44 đến 45% tác vụ sinh code bằng AI trong bộ thử nghiệm của họ có lỗ hổng bảo mật rủi ro, và con số này ít thay đổi theo thời gian.
- Một câu lệnh SQL ghép chuỗi từ dữ liệu người dùng cho phép vào tài khoản mà không cần mật khẩu. Đưa dữ liệu qua tham số là cách sửa.
- Vụ Enrichlead cho thấy hai lỗi rất phổ biến ở code do AI viết: kiểm tra quyền chỉ ở phía client và để secret trong code phía client.
- Series đi qua 12 bài, từ tư duy nền tảng đến bảo mật khi dùng AI để viết code.

## Tài liệu tham khảo

- [Veracode GenAI Code Security Report 2026](https://www.veracode.com/blog/2026-genai-code-security-report-ai-risk/)
- [Veracode GenAI Code Security Report 2025](https://www.veracode.com/resources/analyst-reports/2025-genai-code-security-report/)
- [Vibe Graveyard: Enrichlead](https://vibegraveyard.ai/story/enrichlead-vibe-coded-saas-shutdown/)
- [Kaspersky: Security risks of vibe coding and LLM assistants for developers](https://www.kaspersky.com/blog/vibe-coding-2025-risks/54584/)
- [OWASP Top 10:2025](https://owasp.org/Top10/2025/)
- [Tài liệu Python: sqlite3, dùng placeholder cho tham số](https://docs.python.org/3/library/sqlite3.html#sqlite3-placeholders)
