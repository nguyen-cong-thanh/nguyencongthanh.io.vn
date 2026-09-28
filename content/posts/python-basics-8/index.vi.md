+++
title = "Python cơ bản – Bài 8: Hàm"
date = 2026-09-25
draft = false
tags = ["python", "lập trình cơ bản"]
series = ["Python cơ bản"]
series_weight = 8
+++

Chương trình càng dài, bạn càng thấy mình viết đi viết lại những đoạn code giống nhau: in lời chào, ghép họ tên, tính tổng tiền. Mỗi lần cần sửa, bạn lại phải sửa ở tất cả những chỗ đó. *Hàm* giải quyết chuyện này: đặt tên cho một đoạn code, rồi gọi tên đó mỗi khi cần. Bài này giới thiệu cách viết hàm, truyền dữ liệu vào hàm, nhận kết quả trả về, và tách hàm ra file riêng.

<!--more-->

## Bài này dành cho ai

Người mới bắt đầu, đã đọc [bài 7](../python-basics-7/) về `input()` và vòng lặp `while`. Cần Python 3.14.

## Định nghĩa hàm

```python
def greet_user():
    """In một lời chào đơn giản."""
    print("Xin chào!")

greet_user()
```

```text
Xin chào!
```

- Từ khóa `def` báo cho Python biết bạn đang *định nghĩa* một hàm. Sau `def` là tên hàm, cặp ngoặc đơn và dấu `:`. Ngoặc đơn chứa thông tin hàm cần để làm việc. Hàm này không cần gì nên ngoặc để trống.
- Các dòng thụt lề phía dưới là *thân hàm*.
- Dòng `"""In một lời chào đơn giản."""` là *docstring*: một comment mô tả hàm làm gì, đặt trong ba dấu nháy kép. Python và các công cụ dùng docstring để tạo tài liệu cho hàm.
- Dòng `greet_user()` cuối cùng *gọi* hàm. Khi đó Python mới chạy code trong thân hàm.

### Truyền thông tin vào hàm

Thêm `username` vào ngoặc đơn, hàm sẽ chào được bất kỳ ai:

```python
def greet_user(username):
    """In lời chào kèm tên người dùng."""
    print(f"Xin chào {username.title()}!")

greet_user("minh")
greet_user("lan")
```

```text
Xin chào Minh!
Xin chào Lan!
```

### Tham số và đối số

Hai thuật ngữ này hay bị dùng lẫn lộn:

- `username` trong dòng `def greet_user(username):` là *tham số* (parameter): thông tin hàm cần để làm việc.
- `"minh"` trong lời gọi `greet_user("minh")` là *đối số* (argument): giá trị bạn truyền vào hàm khi gọi nó.

Khi gọi hàm, đối số `"minh"` được gán vào tham số `username`.

## Truyền đối số

Một hàm có thể có nhiều tham số, và có nhiều cách truyền đối số cho chúng.

### Đối số theo vị trí

Python ghép đối số với tham số theo đúng thứ tự:

```python
def describe_pet(animal_type, pet_name):
    """In thông tin về một thú cưng."""
    print(f"\nNhà mình có một con {animal_type}.")
    print(f"Con {animal_type} tên là {pet_name.title()}.")

describe_pet("mèo", "mướp")
describe_pet("chó", "vàng")
```

```text

Nhà mình có một con mèo.
Con mèo tên là Mướp.

Nhà mình có một con chó.
Con chó tên là Vàng.
```

Code mô tả thú cưng chỉ viết một lần trong hàm. Mỗi con thú cưng mới chỉ tốn thêm một dòng gọi hàm.

Đảo thứ tự đối số là ra kết quả buồn cười:

```python
def describe_pet(animal_type, pet_name):
    """In thông tin về một thú cưng."""
    print(f"\nNhà mình có một con {animal_type}.")
    print(f"Con {animal_type} tên là {pet_name.title()}.")

describe_pet("mướp", "mèo")
```

```text

Nhà mình có một con mướp.
Con mướp tên là Mèo.
```

Gặp kết quả kiểu này, hãy kiểm tra thứ tự đối số có khớp với thứ tự tham số trong định nghĩa hàm không.

### Đối số theo tên

*Đối số theo tên* (keyword argument) ghi rõ tên tham số khi gọi hàm, nên không lo nhầm thứ tự:

```python
def describe_pet(animal_type, pet_name):
    """In thông tin về một thú cưng."""
    print(f"\nNhà mình có một con {animal_type}.")
    print(f"Con {animal_type} tên là {pet_name.title()}.")

describe_pet(animal_type="mèo", pet_name="mướp")
describe_pet(pet_name="mướp", animal_type="mèo")
```

```text

Nhà mình có một con mèo.
Con mèo tên là Mướp.

Nhà mình có một con mèo.
Con mèo tên là Mướp.
```

Hai lời gọi cho cùng kết quả. Tên phải viết đúng như tên tham số trong định nghĩa hàm.

### Giá trị mặc định

Tham số có thể có *giá trị mặc định*. Khi gọi hàm mà không truyền đối số cho tham số đó, Python dùng giá trị mặc định:

```python
def describe_pet(pet_name, animal_type="chó"):
    """In thông tin về một thú cưng."""
    print(f"\nNhà mình có một con {animal_type}.")
    print(f"Con {animal_type} tên là {pet_name.title()}.")

describe_pet("vàng")
describe_pet("mướp", "mèo")
describe_pet(pet_name="mướp", animal_type="mèo")
```

```text

Nhà mình có một con chó.
Con chó tên là Vàng.

Nhà mình có một con mèo.
Con mèo tên là Mướp.

Nhà mình có một con mèo.
Con mèo tên là Mướp.
```

`pet_name` được chuyển lên đầu danh sách tham số. Tham số có giá trị mặc định phải đứng *sau* mọi tham số không có giá trị mặc định, để Python vẫn ghép đúng các đối số theo vị trí. Như ví dụ trên cho thấy, cùng một hàm có thể gọi theo nhiều cách tương đương. Cách nào dễ hiểu nhất với bạn thì dùng cách đó.

### Lỗi thiếu đối số

Truyền thiếu hoặc thừa đối số sẽ gây lỗi:

```python
def describe_pet(animal_type, pet_name):
    """In thông tin về một thú cưng."""
    print(f"\nNhà mình có một con {animal_type}.")
    print(f"Con {animal_type} tên là {pet_name.title()}.")

describe_pet()
```

```text
Traceback (most recent call last):
  File "/home/ban/python/thu_cung.py", line 6, in <module>
    describe_pet()
    ~~~~~~~~~~~~^^
TypeError: describe_pet() missing 2 required positional arguments: 'animal_type' and 'pet_name'
```

Traceback cho biết lời gọi hàm nằm ở dòng 6, và thiếu hai đối số `animal_type`, `pet_name`. Đặt tên tham số rõ nghĩa thì thông báo lỗi kiểu này cũng dễ hiểu hơn.

## Giá trị trả về

Hàm không nhất thiết phải in ra màn hình. Hàm có thể xử lý dữ liệu rồi dùng câu lệnh `return` gửi kết quả về chỗ gọi hàm. Kết quả đó gọi là *giá trị trả về*.

### Trả về một giá trị

Tên người Việt viết theo thứ tự họ, tên đệm, tên. Hàm dưới đây ghép họ và tên thành họ tên đầy đủ:

```python
def get_formatted_name(first_name, last_name):
    """Trả về họ tên đầy đủ, viết hoa chữ cái đầu."""
    full_name = f"{last_name} {first_name}"
    return full_name.title()

musician = get_formatted_name("duy", "phạm")
print(musician)
```

```text
Phạm Duy
```

Gọi một hàm có giá trị trả về thì cần một biến để nhận giá trị đó, ở đây là `musician`. Với chương trình lưu họ và tên của hàng nghìn người ở hai chỗ riêng, một hàm như thế này rất tiện: cần hiển thị họ tên ở đâu thì gọi hàm ở đó.

### Tham số tùy chọn

Không phải ai cũng có tên đệm. Cho `middle_name` giá trị mặc định là chuỗi rỗng và đặt nó cuối cùng:

```python
def get_formatted_name(first_name, last_name, middle_name=""):
    """Trả về họ tên đầy đủ, viết hoa chữ cái đầu."""
    if middle_name:
        full_name = f"{last_name} {middle_name} {first_name}"
    else:
        full_name = f"{last_name} {first_name}"
    return full_name.title()

musician = get_formatted_name("duy", "phạm")
print(musician)

musician = get_formatted_name("sơn", "trịnh", "công")
print(musician)
```

```text
Phạm Duy
Trịnh Công Sơn
```

Chuỗi khác rỗng tương đương `True` trong điều kiện, chuỗi rỗng tương đương `False`. Vì vậy `if middle_name:` kiểm tra xem người gọi hàm có truyền tên đệm không.

### Trả về dictionary

Hàm có thể trả về bất kỳ kiểu dữ liệu nào, kể cả list và dictionary:

```python
def build_person(first_name, last_name, age=None):
    """Trả về một dictionary chứa thông tin về một người."""
    person = {"first": first_name, "last": last_name}
    if age:
        person["age"] = age
    return person

print(build_person("an", "nguyễn"))
print(build_person("an", "nguyễn", age=25))
```

```text
{'first': 'an', 'last': 'nguyễn'}
{'first': 'an', 'last': 'nguyễn', 'age': 25}
```

`None` là giá trị đặc biệt nghĩa là "không có giá trị", và tương đương `False` trong điều kiện. Nó hay được dùng làm giá trị mặc định cho tham số tùy chọn.

### Dùng hàm trong vòng lặp while

Kết hợp hàm với `while` để chào người dùng cho tới khi họ muốn dừng. Mỗi lần hỏi đều cho người dùng một cách thoát:

<!-- stdin: an\nnguyễn\nq -->
```python
def get_formatted_name(first_name, last_name):
    """Trả về họ tên đầy đủ, viết hoa chữ cái đầu."""
    full_name = f"{last_name} {first_name}"
    return full_name.title()

while True:
    print("\nCho mình biết tên bạn:")
    print("(gõ 'q' bất cứ lúc nào để thoát)")

    f_name = input("Tên: ")
    if f_name == "q":
        break

    l_name = input("Họ: ")
    if l_name == "q":
        break

    formatted_name = get_formatted_name(f_name, l_name)
    print(f"\nChào {formatted_name}!")
```

```text

Cho mình biết tên bạn:
(gõ 'q' bất cứ lúc nào để thoát)
Tên: an
Họ: nguyễn

Chào Nguyễn An!

Cho mình biết tên bạn:
(gõ 'q' bất cứ lúc nào để thoát)
Tên: q
```

## Truyền list vào hàm

```python
def greet_users(names):
    """In lời chào cho từng người trong list."""
    for name in names:
        print(f"Xin chào {name.title()}!")

usernames = ["hà", "tùng", "mai"]
greet_users(usernames)
```

```text
Xin chào Hà!
Xin chào Tùng!
Xin chào Mai!
```

### Hàm sửa list

Hàm nhận list thì sửa được list đó, và thay đổi là vĩnh viễn. Ví dụ một xưởng in 3D: các mẫu chờ in nằm trong một list, in xong thì chuyển sang list khác. Tách công việc thành hai hàm, mỗi hàm làm một việc:

```python
def print_models(unprinted_designs, completed_models):
    """
    In lần lượt từng mẫu cho tới khi hết.
    In xong mẫu nào thì chuyển mẫu đó sang completed_models.
    """
    while unprinted_designs:
        current_design = unprinted_designs.pop()
        print(f"Đang in: {current_design}")
        completed_models.append(current_design)

def show_completed_models(completed_models):
    """In danh sách các mẫu đã in xong."""
    print("\nCác mẫu đã in xong:")
    for completed_model in completed_models:
        print(completed_model)

unprinted_designs = ["ốp điện thoại", "móc khóa", "mô hình rồng"]
completed_models = []

print_models(unprinted_designs, completed_models)
show_completed_models(completed_models)
print(unprinted_designs)
```

```text
Đang in: mô hình rồng
Đang in: móc khóa
Đang in: ốp điện thoại

Các mẫu đã in xong:
mô hình rồng
móc khóa
ốp điện thoại
[]
```

Phần chương trình chính chỉ còn vài dòng, đọc là hiểu. Muốn sửa cách in, bạn chỉ sửa trong `print_models()`. Mỗi hàm làm đúng một việc cũng giúp code dễ đọc và dễ sửa hơn một hàm làm nhiều việc.

### Không cho hàm sửa list

Dòng cuối cho thấy `unprinted_designs` đã rỗng. Nếu muốn giữ list gốc, hãy truyền vào hàm một bản sao bằng slice `[:]`:

```python
print_models(unprinted_designs[:], completed_models)
```

Hàm vẫn làm việc bình thường, nhưng trên bản sao. Tuy vậy, trừ khi có lý do cụ thể, cứ truyền list gốc. Tạo bản sao tốn thêm thời gian và bộ nhớ, nhất là với list lớn.

## Số lượng đối số tùy ý

### *args

Đôi khi bạn không biết trước hàm sẽ nhận bao nhiêu đối số. Ví dụ một ổ bánh mì có thể có một hay năm loại nhân. Dấu `*` trước tên tham số gom mọi đối số vào một tuple:

```python
def make_banh_mi(*fillings):
    """In danh sách nhân của ổ bánh mì."""
    print(fillings)

make_banh_mi("pate")
make_banh_mi("pate", "chả lụa", "dưa leo")
```

```text
('pate',)
('pate', 'chả lụa', 'dưa leo')
```

Kết hợp với tham số thường thì tham số có `*` phải đứng cuối. Python ghép các đối số theo vị trí trước, phần còn lại dồn vào tham số cuối:

```python
def make_banh_mi(size, *fillings):
    """Tóm tắt ổ bánh mì sắp làm."""
    print(f"\nLàm bánh mì cỡ {size} với nhân:")
    for filling in fillings:
        print(f"- {filling}")

make_banh_mi("nhỏ", "pate")
make_banh_mi("to", "pate", "chả lụa", "dưa leo")
```

```text

Làm bánh mì cỡ nhỏ với nhân:
- pate

Làm bánh mì cỡ to với nhân:
- pate
- chả lụa
- dưa leo
```

Trong code của người khác, bạn sẽ hay gặp tham số tên `*args`, làm đúng việc này.

### **kwargs

Hai dấu `**` gom mọi đối số theo tên vào một dictionary. Cách này hữu ích khi bạn không biết trước người gọi sẽ truyền thông tin gì, ví dụ hồ sơ người dùng:

```python
def build_profile(first, last, **user_info):
    """Tạo dictionary chứa mọi thông tin về một người dùng."""
    user_info["first_name"] = first
    user_info["last_name"] = last
    return user_info

user_profile = build_profile("an", "nguyễn",
                             location="hà nội",
                             field="vật lý")
print(user_profile)
```

```text
{'location': 'hà nội', 'field': 'vật lý', 'first_name': 'an', 'last_name': 'nguyễn'}
```

Tên quen thuộc cho tham số này là `**kwargs`. Có nhiều cách kết hợp tham số thường, tham số mặc định, `*args` và `**kwargs`. Biết chúng tồn tại là để đọc được code người khác. Còn khi tự viết, hãy chọn cách đơn giản nhất làm được việc.

## Lưu hàm trong module

Có thể tách hàm ra một file riêng, gọi là *module*, rồi *import* vào chương trình chính. Chương trình chính sẽ gọn hơn, và bạn dùng lại được hàm ở nhiều chương trình khác nhau. Cũng nhờ import mà bạn dùng được thư viện do người khác viết.

### Import cả module

Module là một file `.py`. Tạo file `banh_mi.py` chỉ chứa hàm `make_banh_mi()`:

<!-- file: banh_mi.py -->
```python
def make_banh_mi(size, *fillings):
    """Tóm tắt ổ bánh mì sắp làm."""
    print(f"\nLàm bánh mì cỡ {size} với nhân:")
    for filling in fillings:
        print(f"- {filling}")
```

Tạo file `order.py` cùng thư mục:

```python
import banh_mi

banh_mi.make_banh_mi("nhỏ", "pate")
banh_mi.make_banh_mi("to", "pate", "chả lụa", "dưa leo")
```

```text

Làm bánh mì cỡ nhỏ với nhân:
- pate

Làm bánh mì cỡ to với nhân:
- pate
- chả lụa
- dưa leo
```

`import banh_mi` cho phép `order.py` dùng mọi hàm trong `banh_mi.py`. Gọi hàm theo cú pháp `ten_module.ten_ham()`.

### Các cách import khác

Import riêng một hoặc vài hàm, khi đó gọi hàm không cần tên module:

```python
from banh_mi import make_banh_mi

make_banh_mi("nhỏ", "pate")
```

```text

Làm bánh mì cỡ nhỏ với nhân:
- pate
```

Đặt *bí danh* (alias) cho hàm hoặc module bằng `as`. Cách này dùng khi tên quá dài hoặc trùng với tên đã có trong chương trình:

```python
from banh_mi import make_banh_mi as mb
import banh_mi as bm

mb("nhỏ", "pate")
bm.make_banh_mi("to", "chả lụa")
```

```text

Làm bánh mì cỡ nhỏ với nhân:
- pate

Làm bánh mì cỡ to với nhân:
- chả lụa
```

Tóm lại có các dạng:

```python
import ten_module
from ten_module import ten_ham
from ten_module import ham_0, ham_1, ham_2
from ten_module import ten_ham as th
import ten_module as tm
```

Bạn có thể gặp thêm dạng `from ten_module import *` để import mọi hàm. Đừng dùng dạng này với module không phải do bạn viết: nếu module có hàm trùng tên với hàm trong chương trình của bạn, hàm này sẽ đè lên hàm kia mà không báo gì.

## Trình bày hàm

Một số quy ước khi viết hàm:

- Tên hàm và tên module dùng chữ thường và dấu gạch dưới, đặt tên nói rõ hàm làm gì.
- Mỗi hàm có docstring ngay dưới dòng `def`, mô tả ngắn gọn hàm làm gì.
- Giá trị mặc định và đối số theo tên không có dấu cách quanh dấu `=`: `def ham(a, b="x")`, `ham(1, b="y")`.
- Dòng `def` quá 79 ký tự thì xuống dòng sau dấu `(`, thụt lề danh sách tham số hai mức để phân biệt với thân hàm.
- Giữa hai hàm để hai dòng trống. Các câu `import` đặt ở đầu file.

Các ví dụ trong bài để một dòng trống giữa các hàm cho gọn.

## Bài tập

1. **Thông điệp:** viết hàm `display_message()` in một câu nói bạn đang học gì trong bài này. Gọi hàm.
2. **Cuốn sách yêu thích:** viết hàm `favorite_book(title)` in câu `Một trong những cuốn sách mình thích là Dế Mèn phiêu lưu ký.`. Gọi hàm với tên một cuốn sách.
3. **In áo:** viết hàm `make_shirt()` nhận cỡ áo và dòng chữ in trên áo, rồi in một câu tóm tắt. Gọi hàm một lần bằng đối số theo vị trí, một lần bằng đối số theo tên. Sau đó cho cỡ mặc định là `L` và dòng chữ mặc định là `Tôi yêu Python`. Làm một áo cỡ L và một áo cỡ M với dòng chữ mặc định, và một áo với dòng chữ khác.
4. **Tỉnh thành:** viết hàm `describe_city()` nhận tên thành phố và tên nước, trong đó tên nước có giá trị mặc định là `"Việt Nam"`. Hàm in ra câu kiểu `Huế thuộc Việt Nam.`. Gọi hàm cho ba thành phố, trong đó một thành phố không thuộc Việt Nam.
5. **Thành phố – quốc gia:** viết hàm `city_country()` nhận tên thành phố và tên nước, *trả về* chuỗi dạng `"Hà Nội, Việt Nam"`. Gọi với ba cặp và in giá trị trả về.
6. **Album:** viết hàm `make_album()` nhận tên ca sĩ và tên album, trả về một dictionary. Thêm tham số tùy chọn số bài hát, mặc định là `None`. Tạo ba album và in ra. Sau đó viết vòng lặp `while` hỏi người dùng tên ca sĩ và tên album, gọi `make_album()` và in kết quả, có cách để người dùng thoát.
7. **Tin nhắn:** tạo list vài tin nhắn ngắn. Viết hàm `show_messages()` in từng tin. Viết thêm hàm `send_messages()` in từng tin rồi chuyển sang list `sent_messages`. In cả hai list sau khi gọi hàm. Gọi lại `send_messages()` với bản sao của list để giữ list gốc.
8. **Bánh mì:** viết hàm nhận số lượng nhân tùy ý và in tóm tắt ổ bánh mì. Gọi hàm ba lần với số nhân khác nhau.
9. **Hồ sơ:** dùng `build_profile()` trong bài để tạo hồ sơ của bạn, thêm ba thông tin bất kỳ.
10. **Xe:** viết hàm `make_car()` nhận hãng, mẫu xe và số lượng tùy ý đối số theo tên, trả về dictionary. Gọi thử: `make_car("vinfast", "vf 8", color="xanh", seats=5)`.
11. **Tách module:** chuyển các hàm của ví dụ in 3D sang file `printing_functions.py` rồi import vào chương trình chính. Với một chương trình khác có hàm, thử lần lượt cả năm cách import trong bài.

## Tóm tắt

- `def ten_ham(tham_so):` định nghĩa hàm. Docstring ngay dưới dòng `def` mô tả hàm làm gì.
- Đối số truyền theo vị trí (đúng thứ tự) hoặc theo tên (`ten=gia_tri`). Tham số có giá trị mặc định đứng sau tham số không có.
- `return` gửi kết quả về chỗ gọi hàm. Tham số tùy chọn thường có mặc định là `""` hoặc `None`.
- Hàm nhận list thì sửa được list gốc. Truyền `list[:]` nếu muốn giữ list gốc.
- `*args` gom đối số theo vị trí vào tuple, `**kwargs` gom đối số theo tên vào dictionary.
- Module là file `.py` chứa hàm. Import bằng `import`, `from ... import ...`, và đặt bí danh bằng `as`.

Bài sau giới thiệu class, cách gom dữ liệu và hàm vào một đối tượng.

## Tài liệu tham khảo

- Eric Matthes, [*Python Crash Course*, 3rd edition](https://nostarch.com/python-crash-course-3rd-edition), chương 8, No Starch Press, 2023.
- [Defining Functions](https://docs.python.org/3/tutorial/controlflow.html#defining-functions) và [More on Defining Functions](https://docs.python.org/3/tutorial/controlflow.html#more-on-defining-functions), Python tutorial.
- [Modules](https://docs.python.org/3/tutorial/modules.html), Python tutorial.
- [PEP 257 – Docstring Conventions](https://peps.python.org/pep-0257/)
