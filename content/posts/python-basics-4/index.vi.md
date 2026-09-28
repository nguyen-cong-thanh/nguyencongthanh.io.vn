+++
title = "Python cơ bản – Bài 4: Làm việc với list"
date = 2026-09-25T09:04:00+07:00
draft = false
tags = ["python", "lập trình cơ bản"]
series = ["Python cơ bản"]
series_weight = 4
+++

Ở bài 3, mỗi lần làm gì với list là mình phải viết index của từng phần tử. List có 3 phần tử thì còn làm được, list có 3000 phần tử thì không. Bài này giới thiệu vòng lặp `for` để xử lý toàn bộ list bằng vài dòng code, cách tạo list số, cách lấy một phần của list, và tuple, một dạng list không sửa được.

<!--more-->

## Bài này dành cho ai

Người mới bắt đầu, đã đọc [bài 3](../python-basics-3/) về list. Cần Python 3.14.

## Duyệt list với vòng lặp for

Giả sử lớp bạn có một tiết mục văn nghệ và bạn muốn in tên từng người biểu diễn:

```python
performers = ["lan", "hùng", "mai"]
for performer in performers:
    print(performer)
```

```text
lan
hùng
mai
```

Dòng `for performer in performers:` bảo Python: lấy lần lượt từng phần tử trong `performers`, gán vào biến `performer`, rồi chạy các dòng thụt lề bên dưới. Đọc thành lời sẽ là "với mỗi người biểu diễn trong danh sách, in tên người đó".

Python chạy như sau:

1. Lấy phần tử đầu tiên `"lan"`, gán vào `performer`, chạy `print(performer)`.
2. Quay lại dòng `for`, lấy phần tử tiếp theo `"hùng"`, lại chạy `print()`.
3. Lặp tiếp với `"mai"`. Hết phần tử thì thoát vòng lặp và chạy dòng tiếp theo sau vòng lặp.

List có một triệu phần tử thì các bước này lặp một triệu lần, và bạn không phải viết thêm dòng nào.

Tên biến trong vòng lặp do bạn chọn. Quy ước là dùng dạng số ít của tên list: `for cat in cats`, `for item in items`. Nhìn vào là biết đâu là một phần tử, đâu là cả list.

### Làm nhiều việc trong vòng lặp

Mọi dòng thụt lề sau `for` đều nằm *trong* vòng lặp và chạy một lần cho mỗi phần tử:

```python
performers = ["lan", "hùng", "mai"]
for performer in performers:
    print(f"{performer.title()} hát hay lắm!")
    print(f"Lần sau diễn tiếp nhé, {performer.title()}.\n")
```

```text
Lan hát hay lắm!
Lần sau diễn tiếp nhé, Lan.

Hùng hát hay lắm!
Lần sau diễn tiếp nhé, Hùng.

Mai hát hay lắm!
Lần sau diễn tiếp nhé, Mai.

```

`\n` ở cuối dòng thứ hai tạo một dòng trống sau mỗi người, để output chia thành từng nhóm.

### Làm việc sau vòng lặp

Dòng không thụt lề sau vòng lặp chỉ chạy một lần, sau khi vòng lặp kết thúc:

```python
performers = ["lan", "hùng", "mai"]
for performer in performers:
    print(f"{performer.title()} hát hay lắm!")

print("Cảm ơn cả nhóm, tiết mục rất hay!")
```

```text
Lan hát hay lắm!
Hùng hát hay lắm!
Mai hát hay lắm!
Cảm ơn cả nhóm, tiết mục rất hay!
```

Cách này hay dùng để tổng kết sau khi đã xử lý xong từng phần tử.

## Lỗi thụt lề

Python dùng thụt lề để biết dòng nào thuộc khối nào. Nhờ vậy code Python dễ đọc, nhưng cũng sinh ra vài lỗi mà người mới hay gặp.

### Quên thụt lề

```python
performers = ["lan", "hùng", "mai"]
for performer in performers:
print(performer)
```

```text
  File "/home/ban/python/van_nghe.py", line 3
    print(performer)
    ^^^^^
IndentationError: expected an indented block after 'for' statement on line 2
```

Sau dòng `for` phải có ít nhất một dòng thụt lề. Sửa bằng cách thụt lề dòng `print()`.

### Quên thụt lề các dòng sau

Lỗi này nguy hiểm hơn, vì chương trình vẫn chạy:

```python
performers = ["lan", "hùng", "mai"]
for performer in performers:
    print(f"{performer.title()} hát hay lắm!")
print(f"Lần sau diễn tiếp nhé, {performer.title()}.")
```

```text
Lan hát hay lắm!
Hùng hát hay lắm!
Mai hát hay lắm!
Lần sau diễn tiếp nhé, Mai.
```

Dòng thứ hai không thụt lề nên chỉ chạy một lần sau vòng lặp. Lúc đó `performer` đang giữ giá trị cuối cùng là `"mai"`, nên chỉ Mai nhận được lời nhắn. Đây là *lỗi logic*: code đúng cú pháp nhưng không làm điều bạn muốn. Thấy một việc lẽ ra lặp nhiều lần mà chỉ chạy một lần, hãy kiểm tra thụt lề.

### Thụt lề không cần thiết

```python
message = "Xin chào!"
    print(message)
```

```text
  File "/home/ban/python/xin_chao.py", line 2
    print(message)
IndentationError: unexpected indent
```

Chỉ thụt lề khi có lý do, ví dụ các dòng nằm trong vòng lặp.

Trường hợp ngược lại, thụt lề nhầm một dòng lẽ ra nằm sau vòng lặp, cũng là lỗi logic: dòng `print("Cảm ơn cả nhóm...")` ở ví dụ trước nếu bị thụt lề sẽ in ba lần, mỗi người một lần. Việc lẽ ra chạy một lần mà chạy nhiều lần thì hãy bỏ thụt lề dòng đó.

### Quên dấu hai chấm

```python
performers = ["lan", "hùng", "mai"]
for performer in performers
    print(performer)
```

```text
  File "/home/ban/python/van_nghe.py", line 2
    for performer in performers
                               ^
SyntaxError: expected ':'
```

Dấu `:` cuối dòng `for` báo cho Python biết khối lệnh bắt đầu. Lần này Python còn gợi ý luôn cách sửa.

## Tạo list số

### Hàm range()

`range()` sinh ra một dãy số:

```python
for value in range(1, 5):
    print(value)
```

```text
1
2
3
4
```

Không có số `5`. `range(a, b)` bắt đầu từ `a` và dừng *trước* `b`. Muốn có số 5 thì dùng `range(1, 6)`. Nếu chỉ truyền một số, dãy bắt đầu từ 0: `range(6)` cho `0` đến `5`.

Truyền `range()` vào `list()` để có một list số. Tham số thứ ba là bước nhảy:

```python
numbers = list(range(1, 6))
print(numbers)

even_numbers = list(range(2, 11, 2))
print(even_numbers)
```

```text
[1, 2, 3, 4, 5]
[2, 4, 6, 8, 10]
```

Kết hợp vòng lặp và `append()` để tạo list phức tạp hơn, ví dụ bình phương của các số từ 1 đến 10:

```python
squares = []
for value in range(1, 11):
    square = value ** 2
    squares.append(square)

print(squares)
```

```text
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

Có thể bỏ biến tạm `square` và viết `squares.append(value ** 2)`. Biến tạm đôi khi giúp code dễ đọc hơn, đôi khi chỉ làm code dài thêm. Cứ viết cách nào bạn hiểu rõ trước, rút gọn sau.

### Thống kê đơn giản

`min()`, `max()` và `sum()` lần lượt trả về giá trị nhỏ nhất, lớn nhất và tổng của một list số:

```python
scores = [7, 9, 5, 10, 8]
print(min(scores))
print(max(scores))
print(sum(scores))
```

```text
5
10
39
```

### List comprehension

*List comprehension* gộp vòng lặp và `append()` vào một dòng. Đây là list bình phương ở trên, viết lại:

```python
squares = [value ** 2 for value in range(1, 11)]
print(squares)
```

```text
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

Đọc từ trái sang: trong ngoặc vuông, đầu tiên là biểu thức tạo giá trị (`value ** 2`), sau đó là vòng lặp cung cấp giá trị cho biểu thức (`for value in range(1, 11)`). Không có dấu `:` ở cuối. Bạn sẽ gặp cú pháp này thường xuyên khi đọc code của người khác. Khi thấy mình viết ba bốn dòng chỉ để tạo một list, hãy thử viết lại bằng list comprehension.

## Làm việc với một phần của list

### Slice

*Slice* là một đoạn của list. Viết index bắt đầu và index kết thúc, cách nhau bằng dấu `:`. Giống `range()`, slice dừng *trước* index kết thúc:

```python
players = ["an", "bình", "cường", "dũng", "đức"]
print(players[0:3])
print(players[1:4])
print(players[:4])
print(players[2:])
print(players[-3:])
```

```text
['an', 'bình', 'cường']
['bình', 'cường', 'dũng']
['an', 'bình', 'cường', 'dũng']
['cường', 'dũng', 'đức']
['cường', 'dũng', 'đức']
```

- Bỏ index đầu thì slice bắt đầu từ đầu list.
- Bỏ index cuối thì slice đi tới hết list.
- Index âm đếm từ cuối: `players[-3:]` là ba người cuối, đúng với list dài bao nhiêu cũng được.

Có thể thêm số thứ ba để chọn bước nhảy, ví dụ `players[::2]` lấy cách một phần tử.

### Duyệt một slice

```python
players = ["an", "bình", "cường", "dũng", "đức"]
print("Ba cầu thủ đá chính:")
for player in players[:3]:
    print(player.title())
```

```text
Ba cầu thủ đá chính:
An
Bình
Cường
```

Slice hữu ích khi bạn cần xử lý dữ liệu theo từng phần: lấy ba điểm cao nhất sau khi sắp xếp, hoặc chia danh sách dài thành nhiều trang để hiển thị.

### Sao chép list

Dùng slice `[:]` không có index nào để tạo một bản sao của cả list:

```python
my_foods = ["phở", "bún bò", "bánh xèo"]
friend_foods = my_foods[:]

my_foods.append("bánh cuốn")
friend_foods.append("chè")

print(my_foods)
print(friend_foods)
```

```text
['phở', 'bún bò', 'bánh xèo', 'bánh cuốn']
['phở', 'bún bò', 'bánh xèo', 'chè']
```

Hai list độc lập: món thêm vào list này không xuất hiện ở list kia. Còn nếu gán thẳng mà không dùng slice:

```python
my_foods = ["phở", "bún bò", "bánh xèo"]
friend_foods = my_foods

my_foods.append("bánh cuốn")
friend_foods.append("chè")

print(my_foods)
print(friend_foods)
```

```text
['phở', 'bún bò', 'bánh xèo', 'bánh cuốn', 'chè']
['phở', 'bún bò', 'bánh xèo', 'bánh cuốn', 'chè']
```

Đây là trường hợp mình nhắc tới ở bài 2: biến là cái nhãn. `friend_foods = my_foods` không tạo list mới mà chỉ dán thêm một cái nhãn lên *cùng một list*. Thêm món qua nhãn nào thì list đó cũng thay đổi. Muốn một bản sao riêng thì dùng `[:]`.

## Tuple

List sửa được, và thường thì đó là điều bạn cần. Nhưng đôi khi bạn muốn một nhóm giá trị không bao giờ thay đổi. Python gọi giá trị không sửa được là *immutable*, và một list immutable gọi là *tuple*.

### Tạo tuple

Tuple giống list, nhưng dùng ngoặc tròn thay vì ngoặc vuông. Truy cập phần tử bằng index như list:

```python
screen = (1920, 1080)
print(screen[0])
print(screen[1])
```

```text
1920
1080
```

Thử sửa một phần tử:

```python
screen = (1920, 1080)
screen[0] = 1280
```

```text
Traceback (most recent call last):
  File "/home/ban/python/man_hinh.py", line 2, in <module>
    screen[0] = 1280
    ~~~~~~^^^
TypeError: 'tuple' object does not support item assignment
```

Python báo lỗi, và đó chính là điều mình muốn: giá trị nào không được đổi thì có lỗi khi ai đó cố đổi.

Thật ra dấu phẩy mới là thứ tạo nên tuple, ngoặc tròn chỉ giúp dễ đọc. Tuple một phần tử phải có dấu phẩy ở cuối: `my_t = (3,)`.

### Duyệt và gán lại tuple

Duyệt tuple bằng `for` giống hệt list. Không sửa được từng phần tử, nhưng bạn có thể gán cả một tuple mới cho biến:

```python
screen = (1920, 1080)
print("Kích thước ban đầu:")
for size in screen:
    print(size)

screen = (1280, 720)
print("\nKích thước mới:")
for size in screen:
    print(size)
```

```text
Kích thước ban đầu:
1920
1080

Kích thước mới:
1280
720
```

Gán lại một biến luôn hợp lệ, nên lần này không có lỗi.

## Viết code dễ đọc

Code được đọc nhiều hơn được viết. Cộng đồng Python có một bộ quy ước trình bày code tên là [PEP 8](https://peps.python.org/pep-0008/). Với những gì bạn đã học đến giờ, cần nhớ ba điều:

- **Thụt lề bằng 4 dấu cách.** Không trộn tab với dấu cách. Các editor đều có tùy chọn để phím Tab chèn 4 dấu cách.
- **Mỗi dòng dưới 80 ký tự.** Phần lớn editor có thể hiện một đường kẻ dọc ở cột 80 để bạn canh.
- **Dùng dòng trống để tách các phần của chương trình**, nhưng đừng dùng quá nhiều. Một dòng trống giữa phần tạo list và phần xử lý list là đủ.

## Bài tập

1. **Món ngon:** lưu ít nhất ba món ăn bạn thích vào list. Dùng `for` để in mỗi món trong một câu, ví dụ `Mình thích ăn phở.`. Sau vòng lặp, in một câu tổng kết như `Món nào cũng ngon!`.
2. **Đếm số:** dùng `for` và `range()` in các số từ 1 đến 20.
3. **Một triệu:** tạo list các số từ 1 đến 1.000.000. Dùng `min()` và `max()` để kiểm tra list bắt đầu từ 1 và kết thúc ở 1.000.000, rồi dùng `sum()` tính tổng.
4. **Số lẻ và bội số của 3:** dùng tham số thứ ba của `range()` để tạo list các số lẻ từ 1 đến 20, và list các bội số của 3 từ 3 đến 30. In từng số bằng `for`.
5. **Lập phương:** tạo list lập phương (`value ** 3`) của các số từ 1 đến 10, một lần bằng vòng lặp và một lần bằng list comprehension.
6. **Slice:** với list món ăn ở bài 1 (thêm món cho đủ ít nhất năm món), in ba món đầu, ba món ở giữa và ba món cuối bằng slice.
7. **Món của mình, món của bạn:** sao chép list món ăn thành `friend_foods`. Thêm một món khác nhau vào mỗi list, rồi dùng `for` in từng list để chứng minh chúng độc lập.
8. **Quán cơm bình dân:** quán chỉ bán năm món, lưu trong một tuple. In từng món bằng `for`. Thử sửa một món để thấy Python báo lỗi. Sau đó quán đổi thực đơn, thay hai món: gán một tuple mới và in lại thực đơn.
9. **Soát lại code:** chọn ba bài ở trên và kiểm tra theo PEP 8: thụt lề 4 dấu cách, dòng dưới 80 ký tự, không lạm dụng dòng trống.

## Tóm tắt

- `for item in items:` chạy các dòng thụt lề bên dưới một lần cho mỗi phần tử. Dòng không thụt lề sau vòng lặp chạy một lần.
- Lỗi thụt lề có thể là lỗi cú pháp (`IndentationError`) hoặc lỗi logic (code chạy nhưng sai kết quả). Đừng quên dấu `:` cuối dòng `for`.
- `range(a, b, step)` sinh dãy số từ `a` đến trước `b`. `list(range(...))` tạo list số. `min()`, `max()`, `sum()` thống kê list số.
- List comprehension: `[bieu_thuc for x in ...]`.
- Slice `list[a:b]` lấy đoạn từ `a` đến trước `b`. `list[:]` tạo bản sao. Gán `b = a` chỉ tạo thêm nhãn cho cùng một list.
- Tuple viết trong `()`, không sửa được từng phần tử nhưng có thể gán lại cả tuple.
- Thụt lề 4 dấu cách, dòng dưới 80 ký tự (PEP 8).

Bài sau giới thiệu câu lệnh `if`, để chương trình làm những việc khác nhau tùy theo điều kiện.

## Tài liệu tham khảo

- Eric Matthes, [*Python Crash Course*, 3rd edition](https://nostarch.com/python-crash-course-3rd-edition), chương 4, No Starch Press, 2023.
- [for Statements](https://docs.python.org/3/tutorial/controlflow.html#for-statements) và [The range() Function](https://docs.python.org/3/tutorial/controlflow.html#the-range-function), Python tutorial.
- [List Comprehensions](https://docs.python.org/3/tutorial/datastructures.html#list-comprehensions) và [Tuples and Sequences](https://docs.python.org/3/tutorial/datastructures.html#tuples-and-sequences), Python tutorial.
- [PEP 8 – Style Guide for Python Code](https://peps.python.org/pep-0008/)
