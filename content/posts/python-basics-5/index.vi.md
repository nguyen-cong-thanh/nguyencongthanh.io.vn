+++
title = "Python cơ bản – Bài 5: Câu lệnh if"
date = 2026-09-25
draft = false
tags = ["python", "lập trình cơ bản"]
series = ["Python cơ bản"]
series_weight = 5
+++

Đến giờ, chương trình của mình luôn chạy giống nhau, từ dòng đầu tới dòng cuối. Nhưng chương trình thực tế phải phản ứng theo tình huống: người dùng nhập sai mật khẩu thì báo lỗi, khách dưới 4 tuổi thì miễn vé, món khách gọi đã hết thì xin lỗi. Bài này giới thiệu điều kiện và câu lệnh `if` để làm việc đó.

<!--more-->

## Bài này dành cho ai

Người mới bắt đầu, đã đọc [bài 4](../python-basics-4/) về vòng lặp `for`. Cần Python 3.14.

## Một ví dụ

Có một list các hãng xe ô tô. Tên hãng thường viết hoa chữ cái đầu, riêng `bmw` phải viết hoa toàn bộ:

```python
cars = ["audi", "bmw", "toyota", "vinfast"]
for car in cars:
    if car == "bmw":
        print(car.upper())
    else:
        print(car.title())
```

```text
Audi
BMW
Toyota
Vinfast
```

Với mỗi xe, chương trình kiểm tra `car == "bmw"`. Đúng thì in chữ hoa, sai thì in kiểu `title()`. Phần còn lại của bài giải thích từng mảnh của ví dụ này.

## Điều kiện

Phần cốt lõi của mọi câu lệnh `if` là một biểu thức cho ra `True` (đúng) hoặc `False` (sai), gọi là *điều kiện*. `True` thì Python chạy khối lệnh bên dưới `if`, `False` thì bỏ qua.

### So sánh bằng và khác

```python
car = "bmw"
print(car == "bmw")
print(car == "audi")
```

```text
True
False
```

Một dấu `=` là *gán*: "đặt giá trị của `car` là `"bmw"`". Hai dấu `==` là *hỏi*: "giá trị của `car` có bằng `"bmw"` không?". Nhầm hai thứ này là lỗi rất hay gặp.

So sánh chuỗi phân biệt chữ hoa, chữ thường. Nếu không quan tâm hoa thường, hãy chuyển về chữ thường trước khi so sánh:

```python
car = "Audi"
print(car == "audi")
print(car.lower() == "audi")
print(car)
```

```text
False
True
Audi
```

`lower()` không sửa giá trị gốc, nên `car` vẫn là `"Audi"`. Các website dùng cách này để đảm bảo tên đăng nhập là duy nhất: `Minh` và `minh` được coi là một.

Để kiểm tra *khác nhau*, dùng `!=`:

```python
requested_topping = "trứng chần"
if requested_topping != "hành":
    print("Không cho hành nhé!")
```

```text
Không cho hành nhé!
```

### So sánh số

Số có đủ các phép so sánh: `==`, `!=`, `<`, `<=`, `>`, `>=`.

```python
age = 17
print(age == 18)
print(age < 18)
print(age <= 18)
print(age > 18)
print(age >= 18)
```

```text
False
True
True
False
False
```

### Kết hợp nhiều điều kiện với and, or

`and` đúng khi *cả hai* điều kiện đều đúng. `or` đúng khi *ít nhất một* điều kiện đúng:

```python
age_0 = 22
age_1 = 17
print(age_0 >= 18 and age_1 >= 18)
print(age_0 >= 18 or age_1 >= 18)

age_1 = 18
print(age_0 >= 18 and age_1 >= 18)
```

```text
False
True
True
```

Có thể đặt từng điều kiện trong ngoặc cho dễ đọc: `(age_0 >= 18) and (age_1 >= 18)`. Ngoặc không bắt buộc.

### Kiểm tra giá trị có trong list không

Dùng `in` và `not in`:

```python
requested_toppings = ["hành", "trứng chần", "quẩy"]
print("quẩy" in requested_toppings)
print("gầu" in requested_toppings)

banned_users = ["spammer01", "troll99"]
user = "minh"
if user not in banned_users:
    print(f"{user.title()} được phép bình luận.")
```

```text
True
False
Minh được phép bình luận.
```

Cách này tiện khi bạn có một danh sách giá trị đặc biệt và cần kiểm tra một giá trị có nằm trong đó không, ví dụ tên đăng nhập đã có người dùng chưa.

### Giá trị boolean

`True` và `False` là *giá trị boolean*, và điều kiện còn được gọi là *biểu thức boolean*. Giá trị boolean thường được lưu vào biến để theo dõi trạng thái, ví dụ game có đang chạy không, người dùng có quyền sửa bài không:

```python
game_active = True
can_edit = False
```

## Câu lệnh if

### if đơn giản

Một điều kiện, một hành động:

```python
age = 19
if age >= 18:
    print("Bạn đủ tuổi thi bằng lái xe máy.")
    print("Bạn đã đăng ký thi chưa?")
```

```text
Bạn đủ tuổi thi bằng lái xe máy.
Bạn đã đăng ký thi chưa?
```

Giống vòng lặp `for`, mọi dòng thụt lề sau `if` thuộc khối lệnh của `if`. Điều kiện sai thì cả khối bị bỏ qua. Với `age = 15`, chương trình này không in gì.

### if-else

`else` là việc cần làm khi điều kiện sai:

```python
age = 15
if age >= 18:
    print("Bạn đủ tuổi thi bằng lái xe máy.")
else:
    print("Bạn chưa đủ tuổi thi bằng lái xe máy.")
    print("Đủ 18 tuổi rồi quay lại nhé!")
```

```text
Bạn chưa đủ tuổi thi bằng lái xe máy.
Đủ 18 tuổi rồi quay lại nhé!
```

Với `if-else`, luôn có đúng một trong hai khối được chạy.

### Chuỗi if-elif-else

Khi có nhiều hơn hai trường hợp, dùng `elif` (viết tắt của *else if*). Ví dụ một khu vui chơi bán vé theo độ tuổi:

- Dưới 4 tuổi: miễn phí.
- Từ 4 đến dưới 18 tuổi: 50.000 đồng.
- Từ 18 tuổi trở lên: 100.000 đồng.

```python
age = 12
if age < 4:
    price = 0
elif age < 18:
    price = 50_000
else:
    price = 100_000

print(f"Giá vé của bạn là {price} đồng.")
```

```text
Giá vé của bạn là 50000 đồng.
```

Python kiểm tra lần lượt từ trên xuống và dừng ở điều kiện *đầu tiên* đúng:

- `age < 4` sai, chuyển xuống `elif`.
- `age < 18` đúng, gán `price = 50_000` rồi bỏ qua phần còn lại, kể cả `else`.
- `else` chỉ chạy khi mọi điều kiện phía trên đều sai.

Khối `if-elif-else` ở đây chỉ làm một việc là xác định giá vé, còn `print()` nằm ngoài và chạy một lần. So với việc đặt `print()` trong từng nhánh, cách này gọn hơn và dễ sửa hơn: muốn đổi câu thông báo, bạn chỉ sửa một chỗ.

### Nhiều elif, và bỏ else

Có thể dùng bao nhiêu `elif` tùy ý. Giả sử khu vui chơi giảm giá cho người từ 65 tuổi:

```python
age = 70
if age < 4:
    price = 0
elif age < 18:
    price = 50_000
elif age < 65:
    price = 100_000
elif age >= 65:
    price = 50_000

print(f"Giá vé của bạn là {price} đồng.")
```

```text
Giá vé của bạn là 50000 đồng.
```

Ở đây mình dùng `elif age >= 65` thay cho `else`. Python không bắt buộc phải có `else`. `else` bắt *mọi* trường hợp còn lại, kể cả dữ liệu sai hoặc không lường trước. Khi trường hợp cuối cùng có điều kiện rõ ràng, viết hẳn điều kiện đó ra bằng `elif` sẽ an toàn hơn.

### Kiểm tra nhiều điều kiện độc lập

`if-elif-else` dừng ngay khi gặp điều kiện đúng đầu tiên. Nếu nhiều điều kiện có thể cùng đúng và bạn muốn xử lý *tất cả*, hãy dùng nhiều câu `if` riêng biệt:

```python
requested_toppings = ["trứng chần", "quẩy"]

if "trứng chần" in requested_toppings:
    print("Thêm trứng chần.")
if "hành" in requested_toppings:
    print("Thêm hành.")
if "quẩy" in requested_toppings:
    print("Thêm quẩy.")

print("\nPhở của bạn đây!")
```

```text
Thêm trứng chần.
Thêm quẩy.

Phở của bạn đây!
```

Nếu đổi hai câu `if` sau thành `elif`, Python sẽ dừng sau "Thêm trứng chần." và khách mất phần quẩy:

```python
requested_toppings = ["trứng chần", "quẩy"]

if "trứng chần" in requested_toppings:
    print("Thêm trứng chần.")
elif "hành" in requested_toppings:
    print("Thêm hành.")
elif "quẩy" in requested_toppings:
    print("Thêm quẩy.")

print("\nPhở của bạn đây!")
```

```text
Thêm trứng chần.

Phở của bạn đây!
```

Tóm lại: chỉ muốn chạy một khối thì dùng `if-elif-else`, muốn chạy nhiều khối thì dùng nhiều `if`.

## Dùng if với list

### Xử lý phần tử đặc biệt

Quán phở in thông báo mỗi khi thêm một món vào tô. Nếu hết gầu thì phải báo khách:

```python
requested_toppings = ["tái", "gầu", "trứng chần"]

for requested_topping in requested_toppings:
    if requested_topping == "gầu":
        print("Xin lỗi, hôm nay quán hết gầu.")
    else:
        print(f"Thêm {requested_topping}.")

print("\nPhở của bạn đây!")
```

```text
Thêm tái.
Xin lỗi, hôm nay quán hết gầu.
Thêm trứng chần.

Phở của bạn đây!
```

### Kiểm tra list có rỗng không

Khi list do người dùng tạo ra, bạn không thể chắc nó có phần tử. Đặt tên list vào `if`: list có ít nhất một phần tử thì điều kiện là `True`, list rỗng thì `False`:

```python
requested_toppings = []

if requested_toppings:
    for requested_topping in requested_toppings:
        print(f"Thêm {requested_topping}.")
    print("\nPhở của bạn đây!")
else:
    print("Bạn muốn ăn phở không người lái à?")
```

```text
Bạn muốn ăn phở không người lái à?
```

### Dùng nhiều list

Khách có thể gọi cả những thứ quán không có. Kiểm tra từng món khách gọi với danh sách món quán có:

```python
available_toppings = ["tái", "chín", "gầu", "nạm", "trứng chần", "quẩy", "hành"]
requested_toppings = ["tái", "phô mai", "quẩy"]

for requested_topping in requested_toppings:
    if requested_topping in available_toppings:
        print(f"Thêm {requested_topping}.")
    else:
        print(f"Xin lỗi, quán không có {requested_topping}.")

print("\nPhở của bạn đây!")
```

```text
Thêm tái.
Xin lỗi, quán không có phô mai.
Thêm quẩy.

Phở của bạn đây!
```

Nếu thực đơn của quán cố định, `available_toppings` có thể là một tuple.

## Trình bày câu lệnh if

PEP 8 chỉ có một khuyến nghị cho điều kiện: đặt một dấu cách hai bên toán tử so sánh. Viết `if age < 4:` thay vì `if age<4:`. Khoảng trắng không ảnh hưởng tới cách Python chạy, chỉ giúp code dễ đọc hơn.

## Bài tập

1. **Dự đoán kết quả:** viết ít nhất 10 điều kiện, 5 cái cho `True` và 5 cái cho `False`. Trước mỗi điều kiện, in ra dự đoán của bạn:

   ```python
   car = "vinfast"
   print("car == 'vinfast'? Mình đoán True.")
   print(car == "vinfast")
   ```

   ```text
   car == 'vinfast'? Mình đoán True.
   True
   ```

   Các điều kiện nên có đủ: so sánh bằng và khác với chuỗi, dùng `lower()`, so sánh số (`==`, `!=`, `<`, `>`, `<=`, `>=`), `and` và `or`, `in` và `not in`.

2. **Đèn giao thông:** gán cho biến `light` một trong ba giá trị `"xanh"`, `"vàng"`, `"đỏ"`.
   - Viết câu `if`: nếu đèn xanh thì in `Được đi.`. Chạy thử với một giá trị làm điều kiện đúng và một giá trị làm điều kiện sai.
   - Đổi thành `if-else`: đèn xanh thì `Được đi.`, còn lại thì `Dừng lại.`.
   - Đổi thành `if-elif-else`: xanh thì `Được đi.`, vàng thì `Giảm tốc độ.`, đỏ thì `Dừng lại.`. Chạy thử cả ba màu.
3. **Các giai đoạn cuộc đời:** gán tuổi cho biến `age` và dùng `if-elif-else` để in ra giai đoạn tương ứng: dưới 2 tuổi là em bé, 2 đến dưới 4 là trẻ mới biết đi, 4 đến dưới 13 là thiếu nhi, 13 đến dưới 20 là thiếu niên, 20 đến dưới 65 là người trưởng thành, từ 65 là người cao tuổi.
4. **Trái cây yêu thích:** tạo list `favorite_fruits` gồm ba loại trái cây, ví dụ xoài, sầu riêng, chôm chôm. Viết năm câu `if` độc lập, mỗi câu kiểm tra một loại trái cây. Loại nào có trong list thì in một câu, ví dụ `Bạn mê sầu riêng thật đấy!`.
5. **Chào admin:** tạo list ít nhất năm tên đăng nhập, trong đó có `"admin"`. Duyệt list và chào từng người: `admin` thì in `Chào admin, bạn có muốn xem báo cáo hệ thống không?`, người khác thì in `Chào Minh, cảm ơn bạn đã quay lại.`. Thêm một câu `if` kiểm tra list rỗng: nếu rỗng thì in `Cần tìm thêm người dùng!`. Xóa hết tên trong list để kiểm tra.
6. **Tên đăng nhập trùng:** tạo list `current_users` gồm năm tên và list `new_users` gồm năm tên, trong đó một hai tên trùng với `current_users`. Với mỗi tên trong `new_users`, in thông báo tên đã có người dùng hoặc tên còn trống. So sánh không phân biệt hoa thường: đã có `Minh` thì không chấp nhận `MINH`. Gợi ý: tạo một list chứa các tên trong `current_users` ở dạng chữ thường.
7. **Xếp hạng:** lưu các số từ 1 đến 9 trong một list. Duyệt list và dùng `if-elif-else` để in thứ hạng: 1 là `Hạng nhất`, 2 là `Hạng nhì`, 3 là `Hạng ba`, còn lại là `Hạng 4`, `Hạng 5`, ... mỗi hạng một dòng.

## Tóm tắt

- Điều kiện cho ra `True` hoặc `False`. Các toán tử: `==`, `!=`, `<`, `<=`, `>`, `>=`, `and`, `or`, `in`, `not in`.
- `=` là gán, `==` là so sánh. So sánh chuỗi phân biệt hoa thường, dùng `lower()` khi cần bỏ qua.
- `if`, `if-else`, `if-elif-else`: Python chạy khối của điều kiện đúng *đầu tiên* rồi bỏ qua phần còn lại. `else` không bắt buộc.
- Cần xử lý mọi điều kiện đúng thì dùng nhiều câu `if` độc lập.
- List rỗng tương đương `False` trong điều kiện.

Bài sau giới thiệu dictionary, cách lưu dữ liệu theo cặp khóa – giá trị.

## Tài liệu tham khảo

- Eric Matthes, [*Python Crash Course*, 3rd edition](https://nostarch.com/python-crash-course-3rd-edition), chương 5, No Starch Press, 2023.
- [if Statements](https://docs.python.org/3/tutorial/controlflow.html#if-statements), Python tutorial.
- [Comparisons](https://docs.python.org/3/library/stdtypes.html#comparisons) và [Truth Value Testing](https://docs.python.org/3/library/stdtypes.html#truth-value-testing), Python documentation.
