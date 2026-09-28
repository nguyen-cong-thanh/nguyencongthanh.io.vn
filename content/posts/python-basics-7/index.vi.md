+++
title = "Python cơ bản – Bài 7: Nhập dữ liệu và vòng lặp while"
date = 2026-09-25T09:07:00+07:00
draft = false
tags = ["python", "lập trình cơ bản"]
series = ["Python cơ bản"]
series_weight = 7
+++

Các chương trình từ đầu series tới giờ đều dùng dữ liệu viết sẵn trong code. Chương trình thực tế thì phải hỏi người dùng: bạn bao nhiêu tuổi, bạn muốn đặt món gì, bạn có muốn tiếp tục không. Bài này giới thiệu hàm `input()` để nhận dữ liệu người dùng nhập vào, và vòng lặp `while` để chương trình chạy tới khi người dùng muốn dừng.

<!--more-->

## Bài này dành cho ai

Người mới bắt đầu, đã đọc [bài 6](../python-basics-6/) về dictionary. Cần Python 3.14.

Các chương trình trong bài chờ người dùng gõ phím, nên hãy chạy chúng trong terminal bằng `python ten_file.py`. Một số editor không nhận được dữ liệu nhập vào khi chạy chương trình bằng nút Run. Trong các khối output bên dưới, phần sau dấu nhắc là thứ người dùng gõ vào.

## Hàm input()

`input()` dừng chương trình, hiện một dòng nhắc và chờ người dùng gõ rồi nhấn Enter. Thứ người dùng gõ được trả về dưới dạng chuỗi:

<!-- stdin: Chào cả nhà! -->
```python
message = input("Nói gì đó, mình sẽ nhắc lại: ")
print(message)
```

```text
Nói gì đó, mình sẽ nhắc lại: Chào cả nhà!
Chào cả nhà!
```

### Viết dòng nhắc rõ ràng

Dòng nhắc nên cho người dùng biết chính xác cần nhập gì. Thêm một dấu cách ở cuối dòng nhắc để chữ người dùng gõ không dính vào dòng nhắc.

Dòng nhắc dài có thể ghép dần vào một biến. Toán tử `+=` nối thêm một chuỗi vào cuối chuỗi đang có:

<!-- stdin: Minh -->
```python
prompt = "Cho mình biết tên để lời chào thân thiện hơn."
prompt += "\nTên bạn là gì? "

name = input(prompt)
print(f"\nChào {name}!")
```

```text
Cho mình biết tên để lời chào thân thiện hơn.
Tên bạn là gì? Minh

Chào Minh!
```

### Nhập số với int()

`input()` luôn trả về chuỗi, kể cả khi người dùng gõ một con số. Đem chuỗi đó đi so sánh với số sẽ gây lỗi:

<!-- stdin: 21 -->
```python
age = input("Bạn bao nhiêu tuổi? ")
print(age >= 18)
```

```text
Bạn bao nhiêu tuổi? 21
Traceback (most recent call last):
  File "/home/ban/python/tuoi.py", line 2, in <module>
    print(age >= 18)
          ^^^^^^^^^
TypeError: '>=' not supported between instances of 'str' and 'int'
```

`age` là chuỗi `"21"`, còn `18` là số, và Python không so sánh được hai kiểu này. Dùng `int()` để chuyển chuỗi thành số nguyên:

<!-- stdin: 135 -->
```python
height = input("Bạn cao bao nhiêu cm? ")
height = int(height)

if height >= 120:
    print("\nBạn đủ chiều cao để chơi tàu lượn!")
else:
    print("\nLớn thêm chút nữa rồi chơi nhé.")
```

```text
Bạn cao bao nhiêu cm? 135

Bạn đủ chiều cao để chơi tàu lượn!
```

Nhớ chuyển dữ liệu nhập vào sang số trước khi tính toán hay so sánh. Nếu người dùng gõ chữ thay vì số, `int()` sẽ báo lỗi. [Bài 10](../python-basics-10/) sẽ nói cách xử lý trường hợp đó.

### Phép chia lấy dư

Toán tử `%` trả về phần dư của phép chia:

```python
print(4 % 3)
print(5 % 3)
print(6 % 3)
print(7 % 3)
```

```text
1
2
0
1
```

Chia hết thì dư `0`. Nhờ vậy bạn kiểm tra được một số là chẵn hay lẻ:

<!-- stdin: 42 -->
```python
number = input("Nhập một số, mình sẽ cho biết số đó chẵn hay lẻ: ")
number = int(number)

if number % 2 == 0:
    print(f"\n{number} là số chẵn.")
else:
    print(f"\n{number} là số lẻ.")
```

```text
Nhập một số, mình sẽ cho biết số đó chẵn hay lẻ: 42

42 là số chẵn.
```

## Vòng lặp while

Vòng lặp `for` chạy một lần cho mỗi phần tử trong một tập hợp. Vòng lặp `while` chạy *chừng nào* điều kiện còn đúng.

```python
current_number = 1
while current_number <= 5:
    print(current_number)
    current_number += 1
```

```text
1
2
3
4
5
```

`current_number += 1` là cách viết tắt của `current_number = current_number + 1`. Mỗi vòng, số tăng thêm 1. Khi số thành 6, điều kiện `current_number <= 5` sai và vòng lặp dừng.

### Để người dùng chọn khi nào dừng

Đặt phần chính của chương trình vào vòng lặp `while`, và dừng khi người dùng gõ một giá trị thoát, ở đây là `thoát`:

<!-- stdin: Chào cả nhà!\nHôm nay trời đẹp.\nthoát -->
```python
prompt = "\nNói gì đó, mình sẽ nhắc lại:"
prompt += "\nGõ 'thoát' để kết thúc. "

message = ""
while message != "thoát":
    message = input(prompt)

    if message != "thoát":
        print(message)
```

```text

Nói gì đó, mình sẽ nhắc lại:
Gõ 'thoát' để kết thúc. Chào cả nhà!
Chào cả nhà!

Nói gì đó, mình sẽ nhắc lại:
Gõ 'thoát' để kết thúc. Hôm nay trời đẹp.
Hôm nay trời đẹp.

Nói gì đó, mình sẽ nhắc lại:
Gõ 'thoát' để kết thúc. thoát
```

`message` được gán chuỗi rỗng `""` trước vòng lặp, để lần đầu tiên Python có giá trị mà so sánh. Câu `if` bên trong giúp chữ `thoát` không bị in ra như một tin nhắn.

### Dùng cờ

Khi có nhiều sự kiện cùng có thể làm chương trình dừng, ví dụ trong game: hết mạng, hết giờ, hoặc người chơi bấm thoát, gom hết vào điều kiện của `while` sẽ rất rối. Thay vào đó, dùng một biến boolean làm *cờ* (flag). Chương trình chạy khi cờ là `True`, và sự kiện nào muốn dừng chương trình thì đặt cờ thành `False`:

<!-- stdin: Chào cả nhà!\nthoát -->
```python
prompt = "\nNói gì đó, mình sẽ nhắc lại:"
prompt += "\nGõ 'thoát' để kết thúc. "

active = True
while active:
    message = input(prompt)

    if message == "thoát":
        active = False
    else:
        print(message)
```

```text

Nói gì đó, mình sẽ nhắc lại:
Gõ 'thoát' để kết thúc. Chào cả nhà!
Chào cả nhà!

Nói gì đó, mình sẽ nhắc lại:
Gõ 'thoát' để kết thúc. thoát
```

Dòng `while` giờ rất đơn giản. Muốn thêm điều kiện dừng, bạn chỉ cần thêm một `elif` đặt `active = False`.

### Thoát vòng lặp với break

`break` thoát khỏi vòng lặp ngay lập tức, bỏ qua phần code còn lại trong vòng lặp. Vòng lặp `while True` chạy mãi cho tới khi gặp `break`:

<!-- stdin: đà lạt\nhội an\nthoát -->
```python
prompt = "\nNhập một tỉnh thành bạn đã đến:"
prompt += "\n(Gõ 'thoát' khi xong.) "

while True:
    city = input(prompt)

    if city == "thoát":
        break
    else:
        print(f"Mình cũng muốn đi {city.title()}!")
```

```text

Nhập một tỉnh thành bạn đã đến:
(Gõ 'thoát' khi xong.) đà lạt
Mình cũng muốn đi Đà Lạt!

Nhập một tỉnh thành bạn đã đến:
(Gõ 'thoát' khi xong.) hội an
Mình cũng muốn đi Hội An!

Nhập một tỉnh thành bạn đã đến:
(Gõ 'thoát' khi xong.) thoát
```

`break` dùng được trong mọi vòng lặp, kể cả `for`.

### Bỏ qua một vòng với continue

`continue` bỏ qua phần còn lại của vòng hiện tại và quay lại đầu vòng lặp. Ví dụ đếm từ 1 đến 10 nhưng chỉ in số lẻ:

```python
current_number = 0
while current_number < 10:
    current_number += 1
    if current_number % 2 == 0:
        continue

    print(current_number)
```

```text
1
3
5
7
9
```

Số chẵn thì `continue` đưa Python về đầu vòng lặp, nên dòng `print()` không chạy.

### Tránh vòng lặp vô hạn

Vòng lặp `while` nào cũng cần có cách dừng. Quên dòng `x += 1` ở đây thì `x` luôn bằng 1, điều kiện luôn đúng, và chương trình in số 1 mãi mãi:

<!-- norun -->
```python
# Vòng lặp này không bao giờ dừng!
x = 1
while x <= 5:
    print(x)
```

Nếu chương trình bị kẹt trong vòng lặp vô hạn, nhấn `Ctrl+C` trong terminal để dừng. Để tránh lỗi này, hãy chạy thử mọi vòng lặp `while` và kiểm tra nó dừng đúng lúc. Nếu chương trình phải dừng khi người dùng gõ một giá trị nào đó, hãy chạy và gõ đúng giá trị đó xem sao.

## while với list và dictionary

Không nên thêm hay xóa phần tử của một list trong lúc đang duyệt nó bằng `for`, vì Python sẽ mất dấu vị trí đang duyệt. Muốn sửa list trong lúc duyệt thì dùng `while`.

### Chuyển phần tử từ list này sang list khác

Một website có danh sách người dùng mới đăng ký, chưa xác minh. Xác minh xong người nào thì chuyển người đó sang danh sách đã xác minh:

```python
# Danh sách cần xác minh và danh sách rỗng để chứa người đã xác minh.
unconfirmed_users = ["an", "bình", "chi"]
confirmed_users = []

# Xác minh từng người cho tới khi hết.
while unconfirmed_users:
    current_user = unconfirmed_users.pop()

    print(f"Đang xác minh: {current_user.title()}")
    confirmed_users.append(current_user)

print("\nĐã xác minh:")
for confirmed_user in confirmed_users:
    print(confirmed_user.title())
```

```text
Đang xác minh: Chi
Đang xác minh: Bình
Đang xác minh: An

Đã xác minh:
Chi
Bình
An
```

`while unconfirmed_users:` chạy chừng nào list còn phần tử. Ở [bài 5](../python-basics-5/) bạn đã biết list rỗng tương đương `False`.

### Xóa mọi lần xuất hiện của một giá trị

Ở [bài 3](../python-basics-3/), `remove()` chỉ xóa lần xuất hiện đầu tiên. Muốn xóa hết thì lặp cho tới khi giá trị không còn trong list:

```python
pets = ["chó", "mèo", "chó", "cá vàng", "mèo", "thỏ", "mèo"]
print(pets)

while "mèo" in pets:
    pets.remove("mèo")

print(pets)
```

```text
['chó', 'mèo', 'chó', 'cá vàng', 'mèo', 'thỏ', 'mèo']
['chó', 'chó', 'cá vàng', 'thỏ']
```

### Lưu dữ liệu nhập vào dictionary

Một chương trình khảo sát: mỗi vòng hỏi tên và câu trả lời, lưu vào dictionary:

<!-- stdin: Minh\nFansipan\ncó\nLan\nTà Xùa\nkhông -->
```python
responses = {}

# Cờ báo khảo sát còn đang chạy.
polling_active = True

while polling_active:
    # Hỏi tên và câu trả lời.
    name = input("\nBạn tên gì? ")
    response = input("Bạn muốn leo ngọn núi nào? ")

    # Lưu câu trả lời vào dictionary.
    responses[name] = response

    # Hỏi xem còn ai muốn trả lời không.
    repeat = input("Còn ai muốn trả lời không? (có/không) ")
    if repeat == "không":
        polling_active = False

# Khảo sát xong, in kết quả.
print("\n--- Kết quả khảo sát ---")
for name, response in responses.items():
    print(f"{name} muốn leo {response}.")
```

```text

Bạn tên gì? Minh
Bạn muốn leo ngọn núi nào? Fansipan
Còn ai muốn trả lời không? (có/không) có

Bạn tên gì? Lan
Bạn muốn leo ngọn núi nào? Tà Xùa
Còn ai muốn trả lời không? (có/không) không

--- Kết quả khảo sát ---
Minh muốn leo Fansipan.
Lan muốn leo Tà Xùa.
```

## Bài tập

1. **Gọi xe:** hỏi người dùng muốn đặt loại xe nào, rồi in ra, ví dụ `Để mình tìm cho bạn một chiếc xe 7 chỗ.`.
2. **Đặt bàn:** hỏi nhóm của người dùng có mấy người. Nhiều hơn 8 người thì báo phải chờ bàn, còn lại thì báo bàn đã sẵn sàng.
3. **Bội số của 10:** hỏi người dùng một số và cho biết số đó có phải bội số của 10 không.
4. **Gọi món thêm:** viết vòng lặp hỏi người dùng muốn thêm gì vào tô phở cho tới khi họ gõ `thoát`. Mỗi lần nhập, in ra `Thêm ... vào tô.`.
5. **Vé xem phim:** rạp bán vé theo tuổi: dưới 3 tuổi miễn phí, từ 3 đến 12 tuổi 50.000 đồng, trên 12 tuổi 90.000 đồng. Viết vòng lặp hỏi tuổi và in giá vé tương ứng.
6. **Ba cách thoát:** viết lại bài 4 hoặc bài 5 theo ba cách, mỗi cách dừng vòng lặp một kiểu: điều kiện trong dòng `while`, biến cờ `active`, và `break` khi người dùng gõ `thoát`.
7. **Vô hạn:** viết một vòng lặp không bao giờ dừng, chạy thử, rồi nhấn `Ctrl+C` để dừng.
8. **Quầy bánh mì:** tạo list `orders` gồm vài loại bánh mì và list rỗng `finished`. Dùng `while` lấy từng đơn ra, in `Xong bánh mì ...`, rồi chuyển sang `finished`. Cuối cùng in danh sách bánh mì đã làm.
9. **Hết pate:** thêm `"bánh mì pate"` vào list `orders` ở bài 8 ít nhất ba lần. Ở đầu chương trình, in thông báo quán đã hết pate, rồi dùng `while` xóa mọi đơn bánh mì pate. Kiểm tra không có bánh mì pate nào trong `finished`.
10. **Kỳ nghỉ mơ ước:** viết chương trình khảo sát kỳ nghỉ mơ ước. Hỏi người dùng muốn đi đâu, ví dụ `Nếu được đi bất cứ đâu, bạn sẽ đi đâu?`, rồi in kết quả khảo sát.

## Tóm tắt

- `input(prompt)` hiện dòng nhắc và trả về chuỗi người dùng gõ vào. Dùng `int()` để chuyển sang số.
- `%` trả về phần dư của phép chia. `n % 2 == 0` nghĩa là `n` chẵn.
- `while` chạy chừng nào điều kiện còn đúng. Có thể dừng bằng điều kiện, biến cờ, hoặc `break`. `continue` bỏ qua phần còn lại của vòng hiện tại.
- Vòng lặp `while` nào cũng cần có cách dừng. Bị kẹt thì nhấn `Ctrl+C`.
- Cần thêm hay xóa phần tử trong lúc duyệt list thì dùng `while` thay vì `for`.

Bài sau giới thiệu hàm, cách gom một đoạn code để dùng lại nhiều lần.

## Tài liệu tham khảo

- Eric Matthes, [*Python Crash Course*, 3rd edition](https://nostarch.com/python-crash-course-3rd-edition), chương 7, No Starch Press, 2023.
- [input()](https://docs.python.org/3/builtins/functions.html#input) và [int()](https://docs.python.org/3/builtins/functions.html#int), Python documentation.
- [The while statement](https://docs.python.org/3/reference/compound_stmts.html#the-while-statement), Python language reference.
- [break and continue Statements](https://docs.python.org/3/tutorial/controlflow.html#break-and-continue-statements), Python tutorial.
