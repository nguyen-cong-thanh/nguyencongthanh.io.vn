+++
title = "Python cơ bản – Bài 10: File và exception"
date = 2026-09-25
draft = false
tags = ["python", "lập trình cơ bản"]
series = ["Python cơ bản"]
series_weight = 10
+++

Mọi chương trình từ đầu series tới giờ đều quên sạch dữ liệu khi kết thúc, và dừng ngay khi gặp lỗi. Chương trình thực tế thì cần đọc dữ liệu từ file, lưu kết quả lại cho lần chạy sau, và không sập chỉ vì người dùng gõ chữ vào chỗ cần nhập số. Bài cuối của series giới thiệu cách đọc ghi file với `pathlib`, xử lý lỗi bằng *exception*, và lưu dữ liệu bằng module `json`.

<!--more-->

## Bài này dành cho ai

Người mới bắt đầu, đã đọc [bài 9](../python-basics-9/) về class. Cần Python 3.14.

Các ví dụ đọc file cần file nằm cùng thư mục với file `.py`. Hãy mở terminal ở thư mục đó rồi mới chạy `python ten_file.py`.

## Đọc file

### Đọc toàn bộ nội dung

Tạo file `nam_quoc_son_ha.txt` chứa bài thơ *Nam quốc sơn hà*, được xem là bản tuyên ngôn độc lập đầu tiên của Việt Nam, ở dạng phiên âm Hán Việt:

<!-- file: nam_quoc_son_ha.txt -->
```text
Nam quốc sơn hà Nam đế cư
Tiệt nhiên định phận tại thiên thư
Như hà nghịch lỗ lai xâm phạm
Nhữ đẳng hành khan thủ bại hư
```

Chương trình đọc và in nội dung file:

```python
from pathlib import Path

path = Path("nam_quoc_son_ha.txt")
contents = path.read_text(encoding="utf-8")
print(contents)
```

```text
Nam quốc sơn hà Nam đế cư
Tiệt nhiên định phận tại thiên thư
Như hà nghịch lỗ lai xâm phạm
Nhữ đẳng hành khan thủ bại hư

```

- Module `pathlib` giúp làm việc với file và thư mục mà không phải bận tâm về khác biệt giữa các hệ điều hành. `Path` là một class trong module này, và `Path("nam_quoc_son_ha.txt")` tạo một đối tượng đại diện cho đường dẫn tới file.
- `read_text()` đọc toàn bộ file và trả về một chuỗi.
- `encoding="utf-8"` báo cho Python biết file được mã hóa bằng UTF-8. Với tiếng Việt có dấu, đừng bỏ tham số này. Trên Windows, mã hóa mặc định thường không phải UTF-8, và thiếu tham số này thì chữ có dấu sẽ bị lỗi hoặc chương trình báo `UnicodeDecodeError`.

Output có thêm một dòng trống ở cuối, vì dòng cuối của file có ký tự xuống dòng và `print()` lại xuống dòng thêm lần nữa. Dùng `rstrip()` để bỏ:

```python
from pathlib import Path

path = Path("nam_quoc_son_ha.txt")
contents = path.read_text(encoding="utf-8").rstrip()
print(contents)
```

```text
Nam quốc sơn hà Nam đế cư
Tiệt nhiên định phận tại thiên thư
Như hà nghịch lỗ lai xâm phạm
Nhữ đẳng hành khan thủ bại hư
```

`read_text(...).rstrip()` gọi hai method nối tiếp nhau: `rstrip()` áp dụng lên chuỗi mà `read_text()` trả về. Cách viết này gọi là *method chaining*.

### Đường dẫn tương đối và tuyệt đối

Khi chỉ truyền tên file, Python tìm file trong thư mục hiện tại của terminal. Nếu file nằm trong thư mục con, dùng *đường dẫn tương đối*:

```python
path = Path("text_files/nam_quoc_son_ha.txt")
```

Hoặc dùng *đường dẫn tuyệt đối*, tức vị trí đầy đủ của file trên máy:

```python
path = Path("/home/ban/data/text_files/nam_quoc_son_ha.txt")
```

Dùng dấu `/` trong đường dẫn kể cả trên Windows. `pathlib` sẽ tự chuyển sang dạng đúng cho từng hệ điều hành.

### Đọc từng dòng

`splitlines()` tách chuỗi thành list các dòng:

```python
from pathlib import Path

path = Path("nam_quoc_son_ha.txt")
contents = path.read_text(encoding="utf-8")

lines = contents.splitlines()
for number, line in enumerate(lines, start=1):
    print(f"Câu {number}: {line}")
```

```text
Câu 1: Nam quốc sơn hà Nam đế cư
Câu 2: Tiệt nhiên định phận tại thiên thư
Câu 3: Như hà nghịch lỗ lai xâm phạm
Câu 4: Nhữ đẳng hành khan thủ bại hư
```

`enumerate()` là hàm có sẵn, trả về từng cặp (số thứ tự, phần tử) khi duyệt list. `start=1` để đếm từ 1 thay vì 0.

### Xử lý nội dung file

Đọc vào rồi thì dữ liệu chỉ là chuỗi, và bạn xử lý như mọi chuỗi khác. Ví dụ kiểm tra bài thơ có đúng thể thất ngôn tứ tuyệt (4 câu, mỗi câu 7 chữ) không:

```python
from pathlib import Path

path = Path("nam_quoc_son_ha.txt")
contents = path.read_text(encoding="utf-8")

lines = contents.splitlines()
print(f"Số câu: {len(lines)}")
for line in lines:
    print(f"{len(line.split())} chữ: {line}")
```

```text
Số câu: 4
7 chữ: Nam quốc sơn hà Nam đế cư
7 chữ: Tiệt nhiên định phận tại thiên thư
7 chữ: Như hà nghịch lỗ lai xâm phạm
7 chữ: Nhữ đẳng hành khan thủ bại hư
```

`split()` không có đối số sẽ tách chuỗi tại mọi khoảng trắng, nên `len(line.split())` là số từ trong dòng.

Mọi thứ đọc từ file đều là chuỗi. Nếu file chứa số và bạn cần tính toán, hãy chuyển bằng `int()` hoặc `float()`.

## Ghi file

### Ghi một dòng

`write_text()` ghi một chuỗi vào file:

```python
from pathlib import Path

path = Path("programming.txt")
path.write_text("Mình thích lập trình.", encoding="utf-8")
```

Chương trình không in gì ra màn hình, nhưng trong thư mục sẽ có file `programming.txt` với nội dung `Mình thích lập trình.`. Nếu file chưa có, `write_text()` tạo mới. Ghi xong, nó tự đóng file đúng cách để dữ liệu không bị mất.

Chỉ ghi được chuỗi vào file văn bản. Muốn ghi số, hãy chuyển sang chuỗi trước bằng `str()`.

### Ghi nhiều dòng

`write_text()` chỉ nhận một chuỗi, nên hãy ghép toàn bộ nội dung vào một chuỗi trước, kèm `\n` ở cuối mỗi dòng:

```python
from pathlib import Path

contents = "Mình thích lập trình.\n"
contents += "Mình thích viết game.\n"
contents += "Mình cũng thích làm việc với dữ liệu.\n"

path = Path("programming.txt")
path.write_text(contents, encoding="utf-8")
print(path.read_text(encoding="utf-8"))
```

```text
Mình thích lập trình.
Mình thích viết game.
Mình cũng thích làm việc với dữ liệu.

```

**Cẩn thận:** nếu file đã tồn tại, `write_text()` xóa toàn bộ nội dung cũ rồi mới ghi. Mục [Lưu dữ liệu](#lưu-dữ-liệu-với-json) bên dưới có cách kiểm tra file đã tồn tại hay chưa.

## Exception

Khi gặp lỗi mà không biết xử lý tiếp thế nào, Python tạo ra một *exception*. Nếu bạn viết code xử lý exception đó, chương trình chạy tiếp. Nếu không, chương trình dừng và in traceback.

### ZeroDivisionError

```python
print(5 / 0)
```

```text
Traceback (most recent call last):
  File "/home/ban/python/chia.py", line 1, in <module>
    print(5 / 0)
          ~~^~~
ZeroDivisionError: division by zero
```

`ZeroDivisionError` là một exception. Biết tên exception rồi, bạn có thể bảo Python phải làm gì khi nó xảy ra.

### Khối try-except

Đặt đoạn code có thể gây lỗi vào `try`, và đoạn xử lý lỗi vào `except`:

```python
try:
    print(5 / 0)
except ZeroDivisionError:
    print("Không chia được cho 0!")
```

```text
Không chia được cho 0!
```

Code trong `try` chạy bình thường thì Python bỏ qua `except`. Nếu code trong `try` gây ra `ZeroDivisionError`, Python chạy khối `except` tương ứng, và chương trình tiếp tục chạy sau đó thay vì dừng.

### Dùng exception để chương trình không sập

Xử lý lỗi đặc biệt quan trọng khi chương trình còn việc phải làm sau khi lỗi xảy ra, ví dụ chương trình nhận dữ liệu từ người dùng. Một máy tính chỉ biết chia, chưa xử lý lỗi:

<!-- stdin: 5\n0 -->
```python
print("Nhập hai số, mình sẽ chia số thứ nhất cho số thứ hai.")
print("Gõ 'q' để thoát.")

while True:
    first_number = input("\nSố thứ nhất: ")
    if first_number == "q":
        break
    second_number = input("Số thứ hai: ")
    if second_number == "q":
        break
    answer = int(first_number) / int(second_number)
    print(answer)
```

```text
Nhập hai số, mình sẽ chia số thứ nhất cho số thứ hai.
Gõ 'q' để thoát.

Số thứ nhất: 5
Số thứ hai: 0
Traceback (most recent call last):
  File "/home/ban/python/chia.py", line 11, in <module>
    answer = int(first_number) / int(second_number)
             ~~~~~~~~~~~~~~~~~~^~~~~~~~~~~~~~~~~~~~
ZeroDivisionError: division by zero
```

Chương trình sập đã là không tốt. Để người dùng thấy traceback còn tệ hơn: người không rành kỹ thuật sẽ bối rối, còn kẻ xấu thì biết thêm về tên file và code của bạn.

### Khối else

Đặt phép chia vào `try-except`. Code chỉ nên chạy khi `try` thành công thì đặt vào `else`:

<!-- stdin: 5\n0\n5\n2\nq -->
```python
print("Nhập hai số, mình sẽ chia số thứ nhất cho số thứ hai.")
print("Gõ 'q' để thoát.")

while True:
    first_number = input("\nSố thứ nhất: ")
    if first_number == "q":
        break
    second_number = input("Số thứ hai: ")
    if second_number == "q":
        break
    try:
        answer = int(first_number) / int(second_number)
    except ZeroDivisionError:
        print("Không chia được cho 0!")
    else:
        print(answer)
```

```text
Nhập hai số, mình sẽ chia số thứ nhất cho số thứ hai.
Gõ 'q' để thoát.

Số thứ nhất: 5
Số thứ hai: 0
Không chia được cho 0!

Số thứ nhất: 5
Số thứ hai: 2
2.5

Số thứ nhất: q
```

- `try` chỉ chứa đúng dòng có thể gây lỗi.
- `except` nói Python phải làm gì khi lỗi xảy ra.
- `else` chứa code chỉ chạy khi `try` thành công.

Người dùng gõ chữ thay vì số thì sao? `int()` sẽ gây ra `ValueError`, và bạn xử lý tương tự. Đó là một bài tập ở cuối bài.

### FileNotFoundError

Lỗi hay gặp khi làm việc với file là file không tồn tại: sai tên, sai thư mục, hoặc file chưa được tạo:

```python
from pathlib import Path

path = Path("hich_tuong_si.txt")
contents = path.read_text(encoding="utf-8")
```

```text
Traceback (most recent call last):
  File "/home/ban/python/doc_file.py", line 4, in <module>
    contents = path.read_text(encoding="utf-8")
  File "/usr/local/lib/python3.14/pathlib/__init__.py", line 787, in read_text
    with self.open(mode='r', encoding=encoding, errors=errors, newline=newline) as f:
         ~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/local/lib/python3.14/pathlib/__init__.py", line 771, in open
    return io.open(self, mode, buffering, encoding, errors, newline)
           ~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
FileNotFoundError: [Errno 2] No such file or directory: 'hich_tuong_si.txt'
```

Traceback này dài hơn, vì lỗi xảy ra sâu bên trong code của `pathlib`. Đường dẫn và số dòng của `pathlib` trên máy bạn có thể khác. Cách đọc vẫn vậy:

- Dòng cuối cho biết loại exception: `FileNotFoundError`. Đây là tên cần đặt sau `except`.
- Tìm lên phía trên dòng đầu tiên thuộc file của bạn, ở đây là dòng 4 của `doc_file.py`. Đó là dòng cần đặt vào `try`.
- Các dòng thuộc thư viện ở giữa thường có thể bỏ qua.

```python
from pathlib import Path

path = Path("hich_tuong_si.txt")
try:
    contents = path.read_text(encoding="utf-8")
except FileNotFoundError:
    print(f"Xin lỗi, không tìm thấy file {path}.")
```

```text
Xin lỗi, không tìm thấy file hich_tuong_si.txt.
```

### Làm việc với nhiều file

Tạo thêm file `tung_gia_hoan_kinh_su.txt` chứa bài *Tụng giá hoàn kinh sư* của Trần Quang Khải, viết sau chiến thắng Chương Dương, Hàm Tử:

<!-- file: tung_gia_hoan_kinh_su.txt -->
```text
Đoạt sáo Chương Dương độ
Cầm Hồ Hàm Tử quan
Thái bình tu trí lực
Vạn cổ thử giang san
```

Viết hàm đếm số chữ trong một file, rồi gọi hàm cho nhiều file. `hich_tuong_si.txt` vẫn chưa được tạo:

```python
from pathlib import Path

def count_words(path):
    """Đếm gần đúng số chữ trong một file."""
    try:
        contents = path.read_text(encoding="utf-8")
    except FileNotFoundError:
        print(f"Xin lỗi, không tìm thấy file {path}.")
    else:
        num_words = len(contents.split())
        print(f"File {path} có khoảng {num_words} chữ.")

filenames = [
    "nam_quoc_son_ha.txt",
    "hich_tuong_si.txt",
    "tung_gia_hoan_kinh_su.txt",
]
for filename in filenames:
    path = Path(filename)
    count_words(path)
```

```text
File nam_quoc_son_ha.txt có khoảng 28 chữ.
Xin lỗi, không tìm thấy file hich_tuong_si.txt.
File tung_gia_hoan_kinh_su.txt có khoảng 20 chữ.
```

Nhờ `try-except`, file bị thiếu không làm dừng cả chương trình: hai file còn lại vẫn được đếm, và người dùng không thấy traceback.

### Bỏ qua lỗi trong im lặng

Không phải exception nào cũng cần báo cho người dùng. Muốn bỏ qua, dùng câu lệnh `pass`, nghĩa là "không làm gì cả":

```python
from pathlib import Path

def count_words(path):
    """Đếm gần đúng số chữ trong một file."""
    try:
        contents = path.read_text(encoding="utf-8")
    except FileNotFoundError:
        pass
    else:
        num_words = len(contents.split())
        print(f"File {path} có khoảng {num_words} chữ.")

filenames = [
    "nam_quoc_son_ha.txt",
    "hich_tuong_si.txt",
    "tung_gia_hoan_kinh_su.txt",
]
for filename in filenames:
    count_words(Path(filename))
```

```text
File nam_quoc_son_ha.txt có khoảng 28 chữ.
File tung_gia_hoan_kinh_su.txt có khoảng 20 chữ.
```

`pass` cũng là lời nhắc rằng bạn cố ý không làm gì ở chỗ này. Sau này bạn có thể thay nó bằng code ghi tên file bị thiếu vào một file log chẳng hạn.

Báo lỗi hay im lặng? Nếu người dùng biết chương trình phải xử lý những file nào, họ sẽ muốn biết vì sao một file bị bỏ qua. Nếu họ chỉ quan tâm tới kết quả, có thể không cần báo. Viết code đúng và được kiểm tra kỹ sẽ ít gặp lỗi nội bộ, nhưng những lỗi đến từ bên ngoài như file bị thiếu hay người dùng nhập sai thì luôn có thể xảy ra, và bạn nên xử lý chúng.

## Lưu dữ liệu với json

Người dùng nhập dữ liệu vào chương trình, và thường muốn dữ liệu đó còn nguyên ở lần chạy sau. Module `json` giúp lưu list, dictionary và các kiểu dữ liệu đơn giản khác vào file, rồi đọc lại. *JSON* (JavaScript Object Notation) là định dạng dữ liệu dạng văn bản, ban đầu dành cho JavaScript nhưng giờ được hầu hết các ngôn ngữ hỗ trợ, nên dữ liệu bạn lưu còn dùng được ở chương trình viết bằng ngôn ngữ khác.

### json.dumps() và json.loads()

`json.dumps()` chuyển dữ liệu Python thành chuỗi JSON, để ghi vào file:

```python
from pathlib import Path
import json

numbers = [2, 3, 5, 7, 11, 13]

path = Path("numbers.json")
contents = json.dumps(numbers)
path.write_text(contents, encoding="utf-8")
```

File `numbers.json` giờ chứa `[2, 3, 5, 7, 11, 13]`. Đuôi `.json` là quy ước cho file chứa dữ liệu JSON. `json.loads()` làm ngược lại: nhận chuỗi JSON, trả về dữ liệu Python:

```python
from pathlib import Path
import json

path = Path("numbers.json")
contents = path.read_text(encoding="utf-8")
numbers = json.loads(contents)
print(numbers)
print(numbers[0] + numbers[1])
```

```text
[2, 3, 5, 7, 11, 13]
5
```

`numbers` là list số thật sự, không phải chuỗi, nên tính toán được ngay.

Với chuỗi tiếng Việt, `json.dumps()` mặc định chuyển chữ có dấu thành mã `\u...`. Dữ liệu vẫn đúng và đọc lại bình thường, chỉ là mở file ra khó đọc. Thêm `ensure_ascii=False` để giữ nguyên chữ:

```python
import json

print(json.dumps("Đức"))
print(json.dumps("Đức", ensure_ascii=False))
```

```text
"\u0110\u1ee9c"
"Đức"
```

### Lưu và đọc dữ liệu người dùng

Chương trình hỏi tên người dùng ở lần chạy đầu tiên, và nhớ tên ở những lần sau. `path.exists()` trả về `True` nếu file đã tồn tại:

<!-- stdin: Minh -->
```python
from pathlib import Path
import json

path = Path("username.json")
if path.exists():
    contents = path.read_text(encoding="utf-8")
    username = json.loads(contents)
    print(f"Chào mừng {username} quay lại!")
else:
    username = input("Bạn tên gì? ")
    contents = json.dumps(username, ensure_ascii=False)
    path.write_text(contents, encoding="utf-8")
    print(f"Lần sau quay lại mình sẽ nhớ tên bạn, {username}!")
```

```text
Bạn tên gì? Minh
Lần sau quay lại mình sẽ nhớ tên bạn, Minh!
```

Chạy lại lần nữa:

```text
Chào mừng Minh quay lại!
```

### Refactoring

Chương trình trên chạy đúng, nhưng mọi việc dồn vào một chỗ. Tách code thành các hàm, mỗi hàm làm một việc, gọi là *refactoring*. Code sau khi refactor dễ đọc, dễ sửa và dễ mở rộng hơn:

<!-- stdin: Lan -->
```python
from pathlib import Path
import json

def get_stored_username(path):
    """Trả về tên đã lưu, hoặc None nếu chưa có."""
    if path.exists():
        contents = path.read_text(encoding="utf-8")
        return json.loads(contents)
    else:
        return None

def get_new_username(path):
    """Hỏi tên người dùng mới và lưu lại."""
    username = input("Bạn tên gì? ")
    contents = json.dumps(username, ensure_ascii=False)
    path.write_text(contents, encoding="utf-8")
    return username

def greet_user():
    """Chào người dùng bằng tên."""
    path = Path("username_v2.json")
    username = get_stored_username(path)
    if username:
        print(f"Chào mừng {username} quay lại!")
    else:
        username = get_new_username(path)
        print(f"Lần sau quay lại mình sẽ nhớ tên bạn, {username}!")

greet_user()
```

```text
Bạn tên gì? Lan
Lần sau quay lại mình sẽ nhớ tên bạn, Lan!
```

- `get_stored_username()` chỉ lo đọc tên đã lưu. Hàm trả về đúng giá trị mong đợi, hoặc `None` nếu không có. Đây là thói quen tốt: nơi gọi hàm chỉ cần kiểm tra `if username:`.
- `get_new_username()` chỉ lo hỏi và lưu tên mới.
- `greet_user()` chỉ lo chào, và gọi hai hàm kia khi cần.

## Bài tập

1. **Học Python:** tạo file `learning_python.txt`, mỗi dòng bắt đầu bằng `Với Python, bạn có thể...` và kể một điều bạn đã học trong series. Viết chương trình đọc file, in toàn bộ nội dung một lần, rồi in lại từng dòng bằng `splitlines()`.
2. **Học C:** method `replace()` thay một từ trong chuỗi bằng từ khác, ví dụ `"Mình thích mèo.".replace("mèo", "chó")` trả về `"Mình thích chó."`. Đọc từng dòng của `learning_python.txt`, thay `Python` bằng tên một ngôn ngữ khác, rồi in ra.
3. **Sổ khách:** viết vòng lặp `while` hỏi tên khách tới chơi. Gom tất cả tên lại, rồi ghi vào file `guest_book.txt`, mỗi tên một dòng.
4. **Phép cộng:** hỏi người dùng hai số, cộng lại và in kết quả. Nếu người dùng gõ chữ thay vì số, `int()` sẽ gây ra `ValueError`. Bắt lỗi này và in thông báo thân thiện. Sau đó đặt chương trình vào vòng lặp `while` để người dùng nhập sai vẫn nhập lại được.
5. **Chó và mèo:** tạo hai file `cats.txt` và `dogs.txt`, mỗi file chứa tên ba con vật. Viết chương trình đọc và in nội dung hai file, bắt `FileNotFoundError` và in thông báo nếu thiếu file. Đổi tên một file để kiểm tra. Sau đó sửa `except` để bỏ qua lỗi trong im lặng.
6. **Đếm từ:** method `count()` đếm số lần một chuỗi xuất hiện trong chuỗi khác, ví dụ `"Hà Nội, hà nội".lower().count("hà nội")` trả về `2`. Đọc một file văn bản dài bất kỳ và đếm số lần một từ xuất hiện. Để ý `count("anh")` cũng đếm cả trong `"thanh"` và `"nhanh"`. Thử đếm `" anh "` có dấu cách hai bên để xem kết quả thay đổi thế nào.
7. **Số yêu thích:** hỏi số yêu thích của người dùng và lưu bằng `json.dumps()`. Viết chương trình khác đọc số đó và in `Mình biết số yêu thích của bạn! Đó là ___.`. Sau đó gộp hai chương trình làm một: đã lưu thì in ra, chưa lưu thì hỏi và lưu.
8. **Nhớ người dùng:** sửa ví dụ refactoring để lưu một dictionary gồm tên, tuổi và thành phố của người dùng thay vì chỉ lưu tên. Khi chào, in lại các thông tin đó. Thêm câu hỏi `Bạn có phải là Minh không?` trước khi chào: nếu không phải thì hỏi thông tin người dùng mới.

## Tóm tắt

- `Path("ten_file")` đại diện cho một đường dẫn. `read_text()` đọc toàn bộ file thành chuỗi, `splitlines()` tách thành list các dòng, `write_text()` ghi đè nội dung file.
- Với tiếng Việt, luôn truyền `encoding="utf-8"` khi đọc ghi file.
- `try-except-else`: `try` chứa code có thể lỗi, `except TenLoi` xử lý lỗi, `else` chạy khi không có lỗi. `pass` để bỏ qua lỗi trong im lặng.
- Đọc traceback từ dòng cuối để biết tên exception, rồi tìm dòng thuộc file của bạn để biết cần đặt `try` ở đâu.
- `json.dumps()` và `json.loads()` chuyển dữ liệu Python sang chuỗi JSON và ngược lại. `ensure_ascii=False` giữ nguyên chữ có dấu. `path.exists()` kiểm tra file đã tồn tại chưa.
- Refactoring tách code thành các hàm, mỗi hàm làm một việc.

Đây là bài cuối của series. Với biến, list, dictionary, `if`, vòng lặp, hàm, class, file và exception, bạn đã có đủ nền tảng để đọc các bài nâng cao hơn trên blog. Nếu muốn đi tiếp theo sách, chương 11 hướng dẫn test code với `pytest`, và Phần II có ba project: một game, một bài trực quan hóa dữ liệu và một web app.

## Tài liệu tham khảo

- Eric Matthes, [*Python Crash Course*, 3rd edition](https://nostarch.com/python-crash-course-3rd-edition), chương 10, No Starch Press, 2023.
- [pathlib — Object-oriented filesystem paths](https://docs.python.org/3/library/pathlib.html), Python documentation.
- [Errors and Exceptions](https://docs.python.org/3/tutorial/errors.html), Python tutorial.
- [json — JSON encoder and decoder](https://docs.python.org/3/library/json.html), Python documentation.
