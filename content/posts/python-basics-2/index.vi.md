+++
title = "Python cơ bản – Bài 2: Biến và kiểu dữ liệu đơn giản"
date = 2026-09-25T09:02:00+07:00
draft = false
tags = ["python", "lập trình cơ bản"]
series = ["Python cơ bản"]
series_weight = 2
+++

Chương trình nào cũng làm việc với dữ liệu: tên người dùng, giá một món hàng, nội dung một tin nhắn. Trước khi viết được thứ gì có ích, bạn cần biết cách lưu những dữ liệu đó lại và xử lý chúng. Bài này giới thiệu biến, hai kiểu dữ liệu đơn giản nhất là chuỗi và số, và cách ghi chú trong code.

<!--more-->

## Bài này dành cho ai

Bài dành cho người mới bắt đầu, đã đọc [bài 1](../python-basics-1/) và cài xong Python 3.14. Mọi ví dụ đều là một file `.py` riêng, chạy bằng lệnh `python ten_file.py` trong terminal.

## Chương trình đầu tiên

Tạo file `xin_chao.py` với nội dung:

```python
print("Xin chào Python!")
```

Chạy file, bạn sẽ thấy:

```text
Xin chào Python!
```

Khi bạn chạy file, *Python interpreter* đọc từng dòng và thực hiện. Gặp `print()`, nó in ra màn hình thứ nằm trong cặp ngoặc. Editor thường tô màu `print` khác với đoạn chữ trong ngoặc kép. Tính năng này gọi là *syntax highlighting*, giúp bạn nhìn ra cấu trúc code dễ hơn.

## Biến

Sửa `xin_chao.py` như sau:

```python
message = "Xin chào Python!"
print(message)

message = "Chào mừng bạn đến với series Python cơ bản!"
print(message)
```

```text
Xin chào Python!
Chào mừng bạn đến với series Python cơ bản!
```

`message` là một *biến*. Dòng đầu gắn biến `message` với giá trị `"Xin chào Python!"`, dòng thứ hai in giá trị đó ra. Sau đó mình gán cho `message` một giá trị mới, và lần `print()` thứ hai in ra giá trị mới. Bạn có thể đổi giá trị của biến bất cứ lúc nào, Python luôn nhớ giá trị hiện tại.

### Đặt tên biến

Có vài quy tắc bắt buộc, vi phạm là lỗi:

- Tên biến chỉ gồm chữ cái, chữ số và dấu gạch dưới `_`, và không được bắt đầu bằng chữ số. `message_1` hợp lệ, `1_message` thì không.
- Không có dấu cách. Dùng `_` để nối các từ: `greeting_message`.
- Không đặt tên trùng với từ khóa hay hàm có sẵn của Python, như `print`, `if`, `for`.

Và vài thói quen nên có:

- Tên ngắn nhưng rõ nghĩa: `student_name` dễ hiểu hơn `s_n`, `name` dễ hiểu hơn `n`.
- Cẩn thận với chữ `l` thường và chữ `O` hoa, vì dễ nhầm với số `1` và số `0`.
- Hiện tại, dùng chữ thường cho tên biến. Chữ in hoa có ý nghĩa riêng, bạn sẽ gặp ở mục hằng số phía dưới.

### Lỗi NameError

Ai viết code cũng gõ sai. Thử cố tình gõ sai tên biến:

```python
message = "Chào bạn đọc!"
print(mesage)
```

```text
Traceback (most recent call last):
  File "/home/ban/python/xin_chao.py", line 2, in <module>
    print(mesage)
          ^^^^^^
NameError: name 'mesage' is not defined. Did you mean: 'message'?
```

Đoạn thông báo này gọi là *traceback*, cho biết chương trình gặp lỗi ở đâu. Đường dẫn file trên máy bạn sẽ khác. Đọc từ dưới lên:

- Dòng cuối cho biết loại lỗi: `NameError`, tức Python không tìm thấy biến tên `mesage`. Python còn gợi ý có thể bạn định gõ `message`.
- Các dòng phía trên chỉ ra lỗi nằm ở dòng 2 của file, và dấu `^^^^^^` đánh dấu đúng chỗ sai.

`NameError` thường do một trong hai nguyên nhân: dùng biến trước khi gán giá trị, hoặc gõ sai tên. Python không kiểm tra chính tả. Nếu bạn gõ sai `mesage` ở cả hai dòng, chương trình vẫn chạy bình thường, vì hai tên khớp nhau.

Rất nhiều lỗi chỉ là gõ sai một ký tự. Mất cả buổi để tìm ra một ký tự như vậy là chuyện bình thường, kể cả với người có nhiều năm kinh nghiệm.

### Biến là cái nhãn

Người ta hay ví biến như cái hộp đựng giá trị. Cách hình dung đó dùng được lúc đầu, nhưng chính xác hơn thì biến là một *cái nhãn* dán lên giá trị, hay nói cách khác là biến *tham chiếu* tới giá trị. Ở các bài đầu, sự khác biệt này chưa quan trọng. Tới bài về list, bạn sẽ gặp trường hợp code chạy khác với mình nghĩ, và hiểu biến là nhãn sẽ giúp bạn giải thích được.

## Chuỗi

*Chuỗi* (string) là một dãy ký tự. Trong Python, mọi thứ nằm giữa hai dấu nháy là chuỗi, dùng nháy đơn hay nháy kép đều được:

```python
"Đây là một chuỗi."
'Đây cũng là một chuỗi.'
```

Nhờ vậy bạn có thể đặt dấu nháy loại này bên trong chuỗi dùng loại kia:

```python
message = "Python's syntax is simple."
print(message)
message = 'Anh ấy nói: "Học Python dễ hơn mình nghĩ."'
print(message)
```

```text
Python's syntax is simple.
Anh ấy nói: "Học Python dễ hơn mình nghĩ."
```

### Đổi chữ hoa, chữ thường

```python
name = "nguyễn văn an"
print(name.title())
print(name.upper())
print(name.lower())
```

```text
Nguyễn Văn An
NGUYỄN VĂN AN
nguyễn văn an
```

`title()`, `upper()` và `lower()` là các *method*: hành động Python thực hiện trên một dữ liệu. Dấu chấm trong `name.title()` nghĩa là "áp dụng `title()` lên `name`". Method luôn có cặp ngoặc đơn phía sau, vì nhiều method cần thêm thông tin để làm việc và thông tin đó nằm trong ngoặc. Ba method ở đây không cần gì thêm nên ngoặc để trống.

- `title()` viết hoa chữ cái đầu mỗi từ, hợp để hiển thị họ tên.
- `upper()` viết hoa toàn bộ.
- `lower()` viết thường toàn bộ. Method này hay được dùng trước khi lưu dữ liệu người dùng nhập, vì không thể trông chờ mọi người gõ hoa thường giống nhau: `An`, `AN` và `an` sau khi `lower()` đều thành `an`.

Các method này xử lý được chữ tiếng Việt có dấu.

### Chèn biến vào chuỗi với f-string

```python
first_name = "an"
last_name = "nguyễn"
full_name = f"{last_name} {first_name}"
print(full_name)
print(f"Chào {full_name.title()}!")
message = f"Chào {full_name.title()}, hôm nay mình học Python nhé!"
print(message)
```

```text
nguyễn an
Chào Nguyễn An!
Chào Nguyễn An, hôm nay mình học Python nhé!
```

Đặt chữ `f` ngay trước dấu nháy mở, rồi đặt tên biến trong cặp ngoặc nhọn `{}`. Khi chạy, Python thay mỗi `{...}` bằng giá trị tương ứng. Kiểu chuỗi này gọi là *f-string*, `f` là viết tắt của *format*. Trong ngoặc nhọn bạn có thể gọi method luôn, như `{full_name.title()}`.

Một ví dụ khác:

```python
island = "Quần đảo Hoàng Sa"
city = "Đà Nẵng"
print(f"{island} thuộc thành phố {city}, Việt Nam.")
```

```text
Quần đảo Hoàng Sa thuộc thành phố Đà Nẵng, Việt Nam.
```

### Tab và xuống dòng

*Whitespace* là các ký tự không in ra chữ: dấu cách, tab, xuống dòng. Dùng `\t` để chèn tab và `\n` để xuống dòng:

```python
print("Python")
print("\tPython")
print("Món ăn sáng:\nPhở\nBún chả\nBánh mì")
print("Món ăn sáng:\n\tPhở\n\tBún chả\n\tBánh mì")
```

```text
Python
	Python
Món ăn sáng:
Phở
Bún chả
Bánh mì
Món ăn sáng:
	Phở
	Bún chả
	Bánh mì
```

Chỉ với một chuỗi, bạn in được nhiều dòng thẳng hàng. Hai bài tới dùng cách này khá nhiều.

### Xóa khoảng trắng thừa

Với người đọc, `"thanhnc"` và `"thanhnc  "` gần như giống nhau. Với Python, đó là hai chuỗi khác nhau. Khi so sánh tên đăng nhập người dùng gõ vào, dấu cách thừa ở cuối có thể khiến đăng nhập thất bại. Python có ba method để xóa khoảng trắng ở hai đầu chuỗi:

```python
username = "  thanhnc  "
print(repr(username.rstrip()))
print(repr(username.lstrip()))
print(repr(username.strip()))
print(repr(username))
```

```text
'  thanhnc'
'thanhnc  '
'thanhnc'
'  thanhnc  '
```

`repr()` in chuỗi kèm dấu nháy để bạn thấy rõ khoảng trắng ở đâu.

- `rstrip()` xóa bên phải.
- `lstrip()` xóa bên trái.
- `strip()` xóa cả hai bên.

Dòng cuối cho thấy `username` không hề thay đổi. Các method này trả về một chuỗi mới, còn chuỗi gốc giữ nguyên. Muốn lưu kết quả, bạn gán lại cho biến:

```python
username = "  thanhnc  "
username = username.strip()
print(repr(username))
```

```text
'thanhnc'
```

### Bỏ tiền tố

`removeprefix()` bỏ một đoạn ở đầu chuỗi, ví dụ bỏ `https://` khỏi một URL:

```python
url = "https://nguyencongthanh.io.vn"
print(url.removeprefix("https://"))
print(url)
```

```text
nguyencongthanh.io.vn
https://nguyencongthanh.io.vn
```

Giống `strip()`, chuỗi gốc không đổi. Muốn giữ kết quả thì gán vào một biến: `domain = url.removeprefix("https://")`.

### Lỗi SyntaxError với dấu nháy

Nếu chuỗi dùng nháy đơn mà bên trong lại có dấu nháy đơn, Python sẽ hiểu chuỗi kết thúc sớm:

```python
message = 'Python's syntax is simple.'
print(message)
```

```text
  File "/home/ban/python/nhay_don.py", line 1
    message = 'Python's syntax is simple.'
                                         ^
SyntaxError: unterminated string literal (detected at line 1)
```

`SyntaxError` nghĩa là Python không đọc được đoạn code vì sai cú pháp. Ở đây, Python coi `'Python'` là một chuỗi, phần `s syntax is simple.` là code, và dấu `'` cuối cùng mở một chuỗi mới không bao giờ được đóng. Cách sửa là dùng nháy kép bên ngoài, như ví dụ đầu mục [Chuỗi](#chuỗi).

Syntax highlighting giúp phát hiện lỗi này sớm: nếu thấy code bị tô màu như chuỗi, hoặc chuỗi bị tô màu như code, nhiều khả năng bạn đang thiếu hoặc thừa một dấu nháy.

## Số

### Số nguyên

Số nguyên (*integer*, kiểu `int`) hỗ trợ cộng `+`, trừ `-`, nhân `*`, chia `/`, và lũy thừa `**`:

```python
print(2 + 3)
print(3 - 2)
print(2 * 3)
print(3 / 2)
print(3 ** 2)
print(10 ** 6)
print(2 + 3 * 4)
print((2 + 3) * 4)
```

```text
5
1
6
1.5
9
1000000
14
20
```

Python tính theo đúng thứ tự ưu tiên như trong toán: nhân chia trước, cộng trừ sau. Dùng ngoặc đơn để đổi thứ tự. Dấu cách quanh phép toán không ảnh hưởng kết quả, chỉ giúp dễ đọc.

### Số thực

Số có dấu chấm thập phân gọi là *float*. Phần lớn thời gian, float cho kết quả như bạn mong đợi, nhưng đôi khi có thêm một dãy chữ số lẻ:

```python
print(0.1 + 0.1)
print(2 * 0.2)
print(0.1 + 0.2)
print(3 * 0.1)
```

```text
0.2
0.4
0.30000000000000004
0.30000000000000004
```

Nguyên nhân là máy tính lưu số thực ở hệ nhị phân, và `0.1` không biểu diễn chính xác được trong hệ này. Ngôn ngữ nào cũng gặp hiện tượng này, không riêng Python. Hiện tại bạn cứ bỏ qua phần lẻ đó. Ai muốn tìm hiểu kỹ có thể đọc [Floating-Point Arithmetic](https://docs.python.org/3/tutorial/floatingpoint.html) trong tài liệu Python.

### Trộn số nguyên và số thực

Phép chia `/` luôn trả về float, kể cả khi chia hết. Phép tính nào có mặt một float thì kết quả cũng là float:

```python
print(4 / 2)
print(1 + 2.0)
print(3.0 ** 2)
```

```text
2.0
3.0
9.0
```

### Dấu gạch dưới trong số, gán nhiều biến, hằng số

```python
motorbike_price = 35_000_000
print(motorbike_price)

x, y, z = 0, 0, 0
print(x, y, z)

MAX_STUDENTS = 40
print(MAX_STUDENTS)
```

```text
35000000
0 0 0
40
```

- **Dấu gạch dưới trong số**: `35_000_000` dễ đọc hơn `35000000`. Python bỏ qua dấu `_` khi lưu số, nên khi in ra chỉ còn các chữ số.
- **Gán nhiều biến trên một dòng**: tên biến cách nhau bằng dấu phẩy, giá trị cũng vậy, và số lượng hai bên phải bằng nhau.
- **Hằng số**: là biến có giá trị không đổi trong suốt chương trình. Python không có kiểu hằng số riêng. Quy ước là viết tên hằng bằng chữ in hoa, như `MAX_STUDENTS`, để báo cho người đọc code rằng không nên gán lại giá trị cho nó.

## Comment

Chương trình càng dài thì càng cần ghi chú để giải thích mình đang làm gì. Trong Python, mọi thứ sau dấu `#` trên một dòng là *comment* và bị interpreter bỏ qua:

```python
# Chào mọi người
print("Xin chào các bạn!")
```

```text
Xin chào các bạn!
```

Comment dùng để giải thích code làm gì và vì sao lại làm như vậy. Khi đang viết, bạn hiểu rõ từng dòng. Vài tháng sau quay lại, rất có thể bạn đã quên. Một cách xác định chỗ cần comment: nếu bạn phải cân nhắc vài cách trước khi chọn được cách làm, hãy ghi lại cách đã chọn và lý do. Xóa bớt comment thừa dễ hơn nhiều so với viết thêm comment cho một chương trình không có ghi chú nào.

Python còn có một bộ nguyên tắc viết code ngắn gọn gọi là *The Zen of Python*. Gõ `python -c "import this"` trong terminal để đọc. Ý chính là ưu tiên code đơn giản, dễ đọc.

## Bài tập

Mỗi bài viết thành một file riêng, đặt tên bằng chữ thường và dấu gạch dưới, ví dụ `loi_nhan.py`.

1. **Lời nhắn:** gán một lời nhắn vào biến rồi in ra. Sau đó gán một lời nhắn khác cho cùng biến đó và in lần nữa.
2. **Rủ bạn đi ăn:** lưu tên một người bạn vào biến và in ra lời rủ, ví dụ `Minh ơi, tối nay đi ăn phở không?`. In thêm tên người đó ở ba dạng: chữ thường, chữ hoa, và hoa chữ cái đầu.
3. **Câu nói nổi tiếng:** lưu tên người nói vào biến `famous_person`, dùng f-string ghép thành câu rồi gán vào biến `message`, sau đó in `message`. Kết quả có dạng:

   ```text
   Chủ tịch Hồ Chí Minh từng nói: "Không có gì quý hơn độc lập, tự do."
   ```

4. **Làm sạch tên:** lưu một cái tên có khoảng trắng ở hai đầu, dùng cả `\t` và `\n`. In tên đó một lần để thấy khoảng trắng, rồi in kết quả của `lstrip()`, `rstrip()` và `strip()`.
5. **Bỏ đuôi file:** Python có method `removesuffix()`, hoạt động giống `removeprefix()` nhưng ở cuối chuỗi. Gán `"bao_cao_thang_9.xlsx"` vào biến `filename`, rồi in tên file không kèm đuôi `.xlsx`.
6. **Tính tiền phở:** một tô phở giá `45_000` đồng. Lưu giá và số tô vào hai biến, rồi in câu `Ăn 3 tô hết 135000 đồng.` bằng f-string. Viết thêm bốn lệnh `print()` dùng bốn phép cộng, trừ, nhân, chia khác nhau mà cùng ra số 8.
7. **Thêm comment:** chọn hai bài ở trên và thêm ít nhất một comment vào mỗi bài, nói chương trình làm gì.

## Tóm tắt

- Biến là cái nhãn gắn với một giá trị. Tên biến dùng chữ thường, số và `_`, không bắt đầu bằng số.
- Chuỗi đặt trong nháy đơn hoặc nháy kép. Các method hay dùng: `title()`, `upper()`, `lower()`, `strip()`, `removeprefix()`. Chúng trả về chuỗi mới và không đổi chuỗi gốc.
- f-string (`f"...{bien}..."`) chèn giá trị biến vào chuỗi.
- Số nguyên và số thực dùng các phép toán `+ - * / **`. Phép `/` luôn ra float. Float có thể có sai số nhỏ.
- Hằng số viết in hoa. Comment bắt đầu bằng `#`.
- Đọc traceback từ dưới lên: dòng cuối là loại lỗi, các dòng trên chỉ vị trí.

Bài sau nói về list, cách lưu nhiều giá trị trong một biến.

## Tài liệu tham khảo

- Eric Matthes, [*Python Crash Course*, 3rd edition](https://nostarch.com/python-crash-course-3rd-edition), chương 2, No Starch Press, 2023.
- [An Informal Introduction to Python](https://docs.python.org/3/tutorial/introduction.html), Python tutorial.
- [String Methods](https://docs.python.org/3/library/stdtypes.html#string-methods), Python documentation.
- [Floating-Point Arithmetic: Issues and Limitations](https://docs.python.org/3/tutorial/floatingpoint.html), Python tutorial.
- [PEP 20 – The Zen of Python](https://peps.python.org/pep-0020/)
