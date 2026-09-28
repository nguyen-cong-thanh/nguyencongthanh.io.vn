+++
title = "Python cơ bản – Bài 3: Làm quen với list"
date = 2026-09-25T09:03:00+07:00
draft = false
tags = ["python", "lập trình cơ bản"]
series = ["Python cơ bản"]
series_weight = 3
+++

Ở bài trước, mỗi biến giữ một giá trị. Nhưng dữ liệu thực tế thường đi theo nhóm: danh sách khách mời, các món trong thực đơn, các tỉnh thành bạn muốn đi. Tạo một biến cho từng thứ thì không ổn. Python có *list* để giữ cả nhóm trong một biến, và bài này giới thiệu cách tạo, đọc, sửa, thêm, xóa và sắp xếp list.

<!--more-->

## Bài này dành cho ai

Người mới bắt đầu, đã đọc [bài 2](../python-basics-2/) về biến, chuỗi và f-string. Cần Python 3.14.

## List là gì

*List* là một dãy phần tử có thứ tự. Phần tử có thể là bất cứ thứ gì: chữ, số, tên người. Trong Python, list được viết trong cặp ngoặc vuông `[]`, các phần tử cách nhau bằng dấu phẩy. Vì list thường chứa nhiều thứ, nên đặt tên list ở dạng số nhiều, như `foods`, `names`, `cities`.

```python
foods = ["phở", "bún chả", "bánh mì", "cơm tấm"]
print(foods)
```

```text
['phở', 'bún chả', 'bánh mì', 'cơm tấm']
```

In cả list thì Python in luôn ngoặc vuông và dấu nháy. Người dùng thường không muốn thấy những thứ đó, nên mình cần lấy từng phần tử ra.

## Truy cập phần tử

Mỗi phần tử có một vị trí, gọi là *index*. Viết tên list kèm index trong ngoặc vuông để lấy phần tử đó:

```python
foods = ["phở", "bún chả", "bánh mì", "cơm tấm"]
print(foods[0])
print(foods[0].title())
```

```text
phở
Phở
```

Phần tử lấy ra là một chuỗi bình thường, nên bạn dùng được các method ở bài 2, như `title()`.

### Index bắt đầu từ 0

Phần tử đầu tiên có index `0`, không phải `1`. Phần lớn ngôn ngữ lập trình đều đếm như vậy. Muốn lấy phần tử thứ *n*, bạn dùng index *n - 1*:

```python
foods = ["phở", "bún chả", "bánh mì", "cơm tấm"]
print(foods[1])
print(foods[3])
```

```text
bún chả
cơm tấm
```

Index âm đếm từ cuối list: `-1` là phần tử cuối, `-2` là phần tử kế cuối, và cứ thế. Cách này tiện khi bạn cần phần tử cuối mà không biết list dài bao nhiêu:

```python
foods = ["phở", "bún chả", "bánh mì", "cơm tấm"]
print(foods[-1])
print(foods[-2])
```

```text
cơm tấm
bánh mì
```

### Dùng phần tử như một biến

Phần tử lấy từ list dùng được ở mọi chỗ mà một biến dùng được, ví dụ trong f-string:

```python
archipelagos = ["Hoàng Sa", "Trường Sa"]
message = f"Quần đảo {archipelagos[0]} và quần đảo {archipelagos[1]} là của Việt Nam."
print(message)
```

```text
Quần đảo Hoàng Sa và quần đảo Trường Sa là của Việt Nam.
```

## Sửa, thêm và xóa phần tử

Phần lớn list sẽ thay đổi trong lúc chương trình chạy: người dùng mới đăng ký thì thêm vào, người dùng hủy tài khoản thì xóa đi. Mục này dùng một list các hãng xe máy làm ví dụ.

### Sửa phần tử

Gán giá trị mới cho phần tử ở một index:

```python
motorcycles = ["honda", "yamaha", "suzuki"]
print(motorcycles)

motorcycles[0] = "vinfast"
print(motorcycles)
```

```text
['honda', 'yamaha', 'suzuki']
['vinfast', 'yamaha', 'suzuki']
```

Chỉ phần tử ở index `0` thay đổi, các phần tử còn lại giữ nguyên.

### Thêm vào cuối list với append()

```python
motorcycles = ["honda", "yamaha", "suzuki"]
motorcycles.append("vinfast")
print(motorcycles)
```

```text
['honda', 'yamaha', 'suzuki', 'vinfast']
```

`append()` hay được dùng để xây list từ một list rỗng. Lúc viết code, bạn thường chưa biết người dùng sẽ nhập gì, nên bắt đầu bằng `[]` rồi thêm dần:

```python
motorcycles = []
motorcycles.append("honda")
motorcycles.append("yamaha")
motorcycles.append("suzuki")
print(motorcycles)
```

```text
['honda', 'yamaha', 'suzuki']
```

### Chèn vào vị trí bất kỳ với insert()

`insert()` nhận index và giá trị. Các phần tử từ vị trí đó trở đi bị đẩy lùi một chỗ:

```python
motorcycles = ["honda", "yamaha", "suzuki"]
motorcycles.insert(0, "vinfast")
print(motorcycles)
```

```text
['vinfast', 'honda', 'yamaha', 'suzuki']
```

### Xóa theo vị trí với del

Nếu biết index của phần tử cần xóa, dùng câu lệnh `del`:

```python
motorcycles = ["honda", "yamaha", "suzuki"]
del motorcycles[1]
print(motorcycles)
```

```text
['honda', 'suzuki']
```

Sau khi `del`, bạn không còn truy cập được giá trị đã xóa.

### Lấy ra và xóa với pop()

Đôi khi bạn cần dùng phần tử ngay sau khi xóa nó khỏi list, ví dụ chuyển một người từ danh sách "đang hoạt động" sang danh sách "đã nghỉ". `pop()` xóa phần tử cuối list và trả về phần tử đó:

```python
motorcycles = ["honda", "yamaha", "suzuki"]
popped_motorcycle = motorcycles.pop()
print(motorcycles)
print(popped_motorcycle)
```

```text
['honda', 'yamaha']
suzuki
```

Cái tên *pop* đến từ hình ảnh một chồng đĩa: lấy cái trên cùng ra. Với list, "trên cùng" là phần tử cuối.

Truyền index vào `pop()` để lấy phần tử ở vị trí bất kỳ:

```python
motorcycles = ["honda", "yamaha", "suzuki"]
first_owned = motorcycles.pop(0)
print(f"Chiếc xe đầu tiên của anh ấy là {first_owned.title()}.")
```

```text
Chiếc xe đầu tiên của anh ấy là Honda.
```

Chọn `del` hay `pop()`? Xóa rồi bỏ luôn thì dùng `del`. Xóa mà còn cần dùng giá trị đó thì dùng `pop()`.

### Xóa theo giá trị với remove()

Khi chỉ biết giá trị mà không biết vị trí, dùng `remove()`:

```python
motorcycles = ["honda", "yamaha", "suzuki", "ducati"]
too_expensive = "ducati"
motorcycles.remove(too_expensive)
print(motorcycles)
print(f"Xe {too_expensive.title()} đắt quá, bỏ khỏi danh sách.")
```

```text
['honda', 'yamaha', 'suzuki']
Xe Ducati đắt quá, bỏ khỏi danh sách.
```

Giá trị đã xóa khỏi list nhưng vẫn còn trong biến `too_expensive`, nên dòng cuối vẫn dùng được.

`remove()` chỉ xóa lần xuất hiện **đầu tiên** của giá trị. Nếu giá trị xuất hiện nhiều lần, bạn cần một vòng lặp, sẽ có ở [bài 7](../python-basics-7/).

## Sắp xếp list

Thứ tự dữ liệu người dùng nhập vào thường lộn xộn, nhưng khi hiển thị bạn lại muốn có thứ tự.

### Sắp xếp vĩnh viễn với sort()

```python
cars = ["toyota", "kia", "vinfast", "hyundai"]
cars.sort()
print(cars)

cars.sort(reverse=True)
print(cars)
```

```text
['hyundai', 'kia', 'toyota', 'vinfast']
['vinfast', 'toyota', 'kia', 'hyundai']
```

`sort()` đổi thứ tự của chính list đó, và không có cách quay lại thứ tự cũ. Truyền `reverse=True` để sắp theo thứ tự ngược.

### Sắp xếp tạm thời với sorted()

Muốn hiển thị theo thứ tự mà vẫn giữ list gốc, dùng hàm `sorted()`:

```python
cars = ["toyota", "kia", "vinfast", "hyundai"]
print(sorted(cars))
print(cars)
```

```text
['hyundai', 'kia', 'toyota', 'vinfast']
['toyota', 'kia', 'vinfast', 'hyundai']
```

`sorted()` cũng nhận `reverse=True`.

Lưu ý: Python so sánh chuỗi theo mã Unicode của từng ký tự, nên chữ hoa, chữ thường và chữ có dấu tiếng Việt không được xếp theo thứ tự từ điển:

```python
foods = ["gà rán", "đậu phụ", "bánh mì"]
print(sorted(foods))
```

```text
['bánh mì', 'gà rán', 'đậu phụ']
```

Theo từ điển tiếng Việt, "đậu phụ" phải đứng trước "gà rán", vì `đ` đứng ngay sau `d`. Nhưng mã Unicode của `đ` lớn hơn mọi chữ cái không dấu, nên Python xếp "đậu phụ" xuống cuối. Sắp xếp tiếng Việt cho đúng cần thêm công cụ khác, nằm ngoài phạm vi series này. Hiện tại, các ví dụ sắp xếp chỉ dùng chữ thường không dấu.

### Đảo ngược thứ tự với reverse()

`reverse()` đảo ngược thứ tự hiện tại của list. Nó không sắp xếp gì cả:

```python
cars = ["toyota", "kia", "vinfast", "hyundai"]
cars.reverse()
print(cars)
```

```text
['hyundai', 'vinfast', 'kia', 'toyota']
```

Thay đổi này là vĩnh viễn, nhưng gọi `reverse()` thêm lần nữa thì list trở về như cũ.

### Độ dài list với len()

```python
cars = ["toyota", "kia", "vinfast", "hyundai"]
print(len(cars))
```

```text
4
```

`len()` đếm từ 1, nên list có bốn phần tử thì `len()` trả về `4`.

## Lỗi IndexError

Lỗi hay gặp nhất với list là truy cập một index không tồn tại:

```python
motorcycles = ["honda", "yamaha", "suzuki"]
print(motorcycles[3])
```

```text
Traceback (most recent call last):
  File "/home/ban/python/xe_may.py", line 2, in <module>
    print(motorcycles[3])
          ~~~~~~~~~~~^^^
IndexError: list index out of range
```

List có ba phần tử, index hợp lệ là `0`, `1`, `2`. Lỗi này thường do quên rằng index bắt đầu từ 0. Gặp `IndexError`, bạn thử lệch index đi một đơn vị.

Để lấy phần tử cuối, dùng `-1` thay vì tự tính index. Cách này đúng kể cả khi list thay đổi độ dài. Trường hợp duy nhất `-1` gây lỗi là khi list rỗng:

```python
motorcycles = []
print(motorcycles[-1])
```

```text
Traceback (most recent call last):
  File "/home/ban/python/xe_may.py", line 2, in <module>
    print(motorcycles[-1])
          ~~~~~~~~~~~^^^^
IndexError: list index out of range
```

Nếu không hiểu vì sao bị `IndexError`, in cả list hoặc in `len()` của list ra xem. List lúc chạy có thể khác xa những gì bạn nghĩ.

## Bài tập

1. **Bạn bè:** lưu tên vài người bạn vào list `names`. In từng tên bằng cách truy cập từng phần tử. Sau đó in lời chào riêng cho từng người, ví dụ `Chào Minh, dạo này khỏe không?`.
2. **Xe mơ ước:** tạo list vài mẫu xe máy hoặc ô tô bạn thích. In mỗi mẫu trong một câu, ví dụ `Mình muốn có một chiếc Honda SH.`.
3. **Liên hoan cuối năm:** bạn tổ chức liên hoan và mời ít nhất ba người.
   - Tạo list khách mời và in lời mời cho từng người.
   - Một người báo bận. In tên người đó, thay họ bằng một người khác trong list, rồi in lại toàn bộ lời mời.
   - Bạn đặt được phòng rộng hơn. Dùng `insert()` thêm một người vào đầu list, một người vào giữa, và dùng `append()` thêm một người vào cuối. In lại lời mời và dùng `len()` để in số khách.
   - Nhà hàng báo chỉ còn chỗ cho hai khách. Dùng `pop()` xóa từng người cho tới khi list còn hai người, mỗi lần xóa thì in lời xin lỗi tới người đó. In lời xác nhận cho hai người còn lại. Cuối cùng dùng `del` xóa nốt hai người và in list để thấy list đã rỗng.
4. **Đi du lịch:** lưu ít nhất năm tỉnh thành bạn muốn đến vào list, tên viết thường không dấu và không theo thứ tự chữ cái (ví dụ `"da lat"`, `"ha giang"`).
   - In list gốc, rồi in kết quả của `sorted()` và `sorted(..., reverse=True)`. Sau mỗi lần, in list gốc để thấy nó không đổi.
   - Dùng `reverse()` hai lần, in list sau mỗi lần.
   - Dùng `sort()` và `sort(reverse=True)`, in list sau mỗi lần.
5. **Cố tình gây lỗi:** sửa index trong một bài ở trên để gây ra `IndexError`, đọc traceback, rồi sửa lại.

## Tóm tắt

- List là dãy phần tử có thứ tự, viết trong `[]`. Index bắt đầu từ `0`, và `-1` là phần tử cuối.
- Sửa: `list[i] = x`. Thêm: `append(x)` vào cuối, `insert(i, x)` vào vị trí `i`.
- Xóa: `del list[i]` theo vị trí, `pop()` hoặc `pop(i)` khi cần dùng lại giá trị, `remove(x)` theo giá trị (chỉ xóa lần xuất hiện đầu).
- `sort()` và `reverse()` đổi list gốc. `sorted()` trả về list mới. `len()` trả về số phần tử.
- `IndexError` nghĩa là index không tồn tại. In list hoặc `len()` ra để kiểm tra.

Bài sau dùng vòng lặp `for` để xử lý mọi phần tử trong list chỉ với vài dòng code.

## Tài liệu tham khảo

- Eric Matthes, [*Python Crash Course*, 3rd edition](https://nostarch.com/python-crash-course-3rd-edition), chương 3, No Starch Press, 2023.
- [More on Lists](https://docs.python.org/3/tutorial/datastructures.html#more-on-lists), Python tutorial.
- [Sorting Techniques](https://docs.python.org/3/howto/sorting.html), Python HOWTOs.
