+++
title = "Python cơ bản – Bài 6: Dictionary"
date = 2026-09-25T09:06:00+07:00
draft = false
tags = ["python", "lập trình cơ bản"]
series = ["Python cơ bản"]
series_weight = 6
+++

List giữ các phần tử theo thứ tự, và bạn lấy phần tử ra bằng vị trí. Nhưng nhiều dữ liệu tự nhiên đi theo cặp: tên món và giá, tên đăng nhập và thông tin người dùng, quần đảo và tỉnh thành quản lý. Với những dữ liệu này, bạn muốn tra theo *tên* chứ không theo vị trí. Python có *dictionary* cho việc đó. Bài này giới thiệu cách tạo, đọc, sửa, duyệt dictionary, và cách lồng dictionary với list.

<!--more-->

## Bài này dành cho ai

Người mới bắt đầu, đã đọc [bài 5](../python-basics-5/) về câu lệnh `if`. Cần Python 3.14.

## Dictionary đầu tiên

Một món trong thực đơn quán trà sữa:

```python
drink = {"name": "trà sữa trân châu", "price": 30_000}
print(drink["name"])
print(drink["price"])
```

```text
trà sữa trân châu
30000
```

*Dictionary* là tập hợp các cặp *khóa – giá trị* (key – value). Mỗi khóa gắn với một giá trị, và bạn dùng khóa để lấy giá trị đó ra. Dictionary viết trong cặp ngoặc nhọn `{}`. Khóa và giá trị cách nhau bằng dấu `:`, các cặp cách nhau bằng dấu phẩy. Giá trị có thể là số, chuỗi, list, thậm chí một dictionary khác.

## Làm việc với dictionary

### Lấy giá trị

Viết tên dictionary rồi đặt khóa trong ngoặc vuông. Giá trị lấy ra dùng như một biến bình thường:

```python
drink = {"name": "trà sữa trân châu", "price": 30_000}
price = drink["price"]
print(f"Một ly {drink['name']} giá {price} đồng.")
```

```text
Một ly trà sữa trân châu giá 30000 đồng.
```

Để ý: f-string dùng nháy kép bên ngoài, nên khóa bên trong dùng nháy đơn `drink['name']`.

### Thêm cặp khóa – giá trị

Gán giá trị cho một khóa mới:

```python
drink = {"name": "trà sữa trân châu", "price": 30_000}
print(drink)

drink["size"] = "L"
drink["sugar"] = "50%"
print(drink)
```

```text
{'name': 'trà sữa trân châu', 'price': 30000}
{'name': 'trà sữa trân châu', 'price': 30000, 'size': 'L', 'sugar': '50%'}
```

Dictionary giữ đúng thứ tự các cặp được thêm vào. Khi in hay duyệt, các cặp xuất hiện theo thứ tự đó.

### Bắt đầu từ dictionary rỗng

Khi dữ liệu do người dùng nhập vào hoặc được sinh tự động, bạn thường bắt đầu với `{}` rồi thêm dần:

```python
drink = {}
drink["name"] = "trà đào"
drink["price"] = 35_000
print(drink)
```

```text
{'name': 'trà đào', 'price': 35000}
```

### Sửa giá trị

Gán giá trị mới cho khóa đã có:

```python
drink = {"name": "trà đào", "price": 35_000}
print(f"Giá cũ: {drink['price']}")

drink["price"] = 40_000
print(f"Giá mới: {drink['price']}")
```

```text
Giá cũ: 35000
Giá mới: 40000
```

Một ví dụ khác: vị trí của một shipper trên bản đồ, mỗi phút đi được bao xa tùy vào phương tiện:

```python
shipper = {"x_position": 0, "vehicle": "xe máy"}
print(f"Vị trí ban đầu: {shipper['x_position']}")

# Quãng đường mỗi phút phụ thuộc vào phương tiện.
if shipper["vehicle"] == "xe đạp":
    x_increment = 1
elif shipper["vehicle"] == "xe máy":
    x_increment = 3
else:
    # Còn lại là đi bộ.
    x_increment = 0.5

shipper["x_position"] = shipper["x_position"] + x_increment
print(f"Vị trí mới: {shipper['x_position']}")
```

```text
Vị trí ban đầu: 0
Vị trí mới: 3
```

Chỉ cần đổi một giá trị trong dictionary, ví dụ `shipper["vehicle"] = "xe đạp"`, là cách di chuyển của shipper thay đổi theo.

### Xóa cặp khóa – giá trị

Dùng `del` với khóa cần xóa. Cặp bị xóa sẽ mất hẳn:

```python
drink = {"name": "trà đào", "price": 35_000, "sugar": "50%"}
del drink["sugar"]
print(drink)
```

```text
{'name': 'trà đào', 'price': 35000}
```

### Dictionary chứa nhiều đối tượng giống nhau

Ví dụ trên lưu nhiều thông tin khác nhau về *một* đối tượng. Dictionary cũng dùng được để lưu *một* loại thông tin về nhiều đối tượng, ví dụ kết quả khảo sát ngôn ngữ lập trình yêu thích:

```python
favorite_languages = {
    "an": "python",
    "bình": "c",
    "chi": "rust",
    "dũng": "python",
}

language = favorite_languages["bình"].title()
print(f"Ngôn ngữ yêu thích của Bình là {language}.")
```

```text
Ngôn ngữ yêu thích của Bình là C.
```

Dictionary dài nên viết thành nhiều dòng: xuống dòng sau `{`, thụt lề mỗi cặp 4 dấu cách, thêm dấu phẩy sau mỗi cặp kể cả cặp cuối, và đặt `}` ở dòng riêng. Thêm cặp mới về sau sẽ dễ hơn.

### Dùng get() khi khóa có thể không tồn tại

Lấy một khóa không có trong dictionary sẽ gây lỗi:

```python
drink = {"name": "trà đào", "price": 35_000}
print(drink["size"])
```

```text
Traceback (most recent call last):
  File "/home/ban/python/tra_sua.py", line 2, in <module>
    print(drink["size"])
          ~~~~~^^^^^^^^
KeyError: 'size'
```

`get()` nhận khóa và một giá trị mặc định. Khóa không tồn tại thì trả về giá trị mặc định thay vì báo lỗi:

```python
drink = {"name": "trà đào", "price": 35_000}
size = drink.get("size", "Chưa chọn size.")
print(size)
print(drink.get("size"))
```

```text
Chưa chọn size.
None
```

Nếu không truyền giá trị mặc định, `get()` trả về `None`. `None` là một giá trị đặc biệt, nghĩa là "không có giá trị". Đó không phải lỗi. Khóa có thể không tồn tại thì nên dùng `get()` thay vì ngoặc vuông.

## Duyệt dictionary

### Duyệt tất cả các cặp

Dùng method `items()` và hai biến trong vòng lặp `for`, một cho khóa, một cho giá trị:

```python
archipelagos = {
    "Hoàng Sa": "thành phố Đà Nẵng",
    "Trường Sa": "tỉnh Khánh Hòa",
}

for archipelago, province in archipelagos.items():
    print(f"Quần đảo {archipelago} thuộc {province}, Việt Nam.")
```

```text
Quần đảo Hoàng Sa thuộc thành phố Đà Nẵng, Việt Nam.
Quần đảo Trường Sa thuộc tỉnh Khánh Hòa, Việt Nam.
```

Tên hai biến tùy bạn chọn, `for k, v in ...items()` cũng chạy. Nhưng tên có nghĩa như `archipelago`, `province` giúp đọc code dễ hơn nhiều.

### Duyệt các khóa

`keys()` trả về các khóa. Bên trong vòng lặp, bạn vẫn lấy được giá trị bằng khóa hiện tại:

```python
favorite_languages = {
    "an": "python",
    "bình": "c",
    "chi": "rust",
    "dũng": "python",
}

friends = ["dũng", "bình"]
for name in favorite_languages.keys():
    print(f"Chào {name.title()}.")
    if name in friends:
        language = favorite_languages[name].title()
        print(f"\t{name.title()}, nghe nói bạn thích {language}!")
```

```text
Chào An.
Chào Bình.
	Bình, nghe nói bạn thích C!
Chào Chi.
Chào Dũng.
	Dũng, nghe nói bạn thích Python!
```

Duyệt dictionary mặc định là duyệt khóa, nên `for name in favorite_languages:` cho kết quả giống hệt. Viết rõ `.keys()` hay không tùy bạn thấy cách nào dễ đọc hơn.

`keys()` cũng dùng được với `in` để kiểm tra một người đã tham gia khảo sát chưa:

```python
favorite_languages = {"an": "python", "bình": "c"}
if "giang" not in favorite_languages.keys():
    print("Giang ơi, làm khảo sát giúp mình nhé!")
```

```text
Giang ơi, làm khảo sát giúp mình nhé!
```

### Duyệt khóa theo thứ tự

Dùng `sorted()` để sắp xếp các khóa trước khi duyệt:

```python
favorite_languages = {"dũng": "python", "an": "python", "chi": "rust"}
for name in sorted(favorite_languages.keys()):
    print(f"{name.title()}, cảm ơn bạn đã tham gia khảo sát.")
```

```text
An, cảm ơn bạn đã tham gia khảo sát.
Chi, cảm ơn bạn đã tham gia khảo sát.
Dũng, cảm ơn bạn đã tham gia khảo sát.
```

### Duyệt các giá trị

`values()` trả về các giá trị, không kèm khóa. Giá trị trùng nhau sẽ xuất hiện nhiều lần. Muốn bỏ trùng, dùng `set()`. *Set* là tập hợp mà mỗi phần tử chỉ xuất hiện một lần:

```python
favorite_languages = {
    "an": "python",
    "bình": "c",
    "chi": "rust",
    "dũng": "python",
}

print("Các ngôn ngữ được nhắc tới:")
for language in favorite_languages.values():
    print(language.title())

print("\nKhông trùng lặp:")
for language in sorted(set(favorite_languages.values())):
    print(language.title())
```

```text
Các ngôn ngữ được nhắc tới:
Python
C
Rust
Python

Không trùng lặp:
C
Python
Rust
```

Set không giữ thứ tự, nên in trực tiếp một set có thể ra thứ tự khác nhau giữa các lần chạy. Ở đây mình dùng `sorted()` để kết quả ổn định.

Set cũng tạo được trực tiếp bằng ngoặc nhọn: `languages = {"python", "rust", "c"}`. Ngoặc nhọn mà không có cặp khóa – giá trị nào thì đó là set, không phải dictionary.

## Lồng nhau

Bạn có thể đặt dictionary trong list, list trong dictionary, hoặc dictionary trong dictionary. Cách này gọi là *lồng nhau* (nesting).

### List chứa dictionary

Một rạp chiếu phim có 30 ghế, mỗi ghế là một dictionary. Tạo cả 30 ghế bằng vòng lặp:

```python
# Tạo list rỗng để chứa các ghế.
seats = []

# Tạo 30 ghế trống.
for seat_number in range(30):
    new_seat = {"status": "trống", "price": 80_000}
    seats.append(new_seat)

# Ba ghế đầu đã có người đặt.
for seat in seats[:3]:
    if seat["status"] == "trống":
        seat["status"] = "đã đặt"

# In 5 ghế đầu.
for seat in seats[:5]:
    print(seat)
print("...")

print(f"Tổng số ghế: {len(seats)}")
```

```text
{'status': 'đã đặt', 'price': 80000}
{'status': 'đã đặt', 'price': 80000}
{'status': 'đã đặt', 'price': 80000}
{'status': 'trống', 'price': 80000}
{'status': 'trống', 'price': 80000}
...
Tổng số ghế: 30
```

`range(30)` ở đây chỉ dùng để lặp 30 lần. Các ghế có giá trị giống nhau lúc tạo, nhưng mỗi ghế là một dictionary riêng, nên sửa ghế này không ảnh hưởng ghế khác. Có thể thêm `elif` để chuyển ghế `"đã đặt"` sang `"đã thanh toán"`.

Khi lưu nhiều dictionary trong một list, hãy cho chúng cùng cấu trúc, tức là có cùng các khóa. Nhờ vậy một vòng lặp xử lý được tất cả.

### Dictionary chứa list

Khi một khóa cần gắn với nhiều giá trị, dùng list làm giá trị:

```python
# Thông tin một ổ bánh mì khách gọi.
banh_mi = {
    "size": "to",
    "fillings": ["pate", "chả lụa", "dưa leo"],
}

print(f"Bạn gọi bánh mì loại {banh_mi['size']} "
    "với nhân:")
for filling in banh_mi["fillings"]:
    print(f"\t{filling}")
```

```text
Bạn gọi bánh mì loại to với nhân:
	pate
	chả lụa
	dưa leo
```

Dòng `print()` quá dài có thể tách làm hai: kết thúc dòng đầu bằng dấu nháy đóng, dòng sau thụt lề và bắt đầu bằng dấu nháy mở. Python tự nối hai chuỗi đứng cạnh nhau thành một.

Quay lại khảo sát ngôn ngữ, giờ mỗi người được chọn nhiều ngôn ngữ:

```python
favorite_languages = {
    "an": ["python", "rust"],
    "bình": ["c"],
    "chi": ["rust", "go"],
}

for name, languages in favorite_languages.items():
    print(f"\nNgôn ngữ yêu thích của {name.title()}:")
    for language in languages:
        print(f"\t{language.title()}")
```

```text

Ngôn ngữ yêu thích của An:
	Python
	Rust

Ngôn ngữ yêu thích của Bình:
	C

Ngôn ngữ yêu thích của Chi:
	Rust
	Go
```

Vòng lặp ngoài duyệt từng người, vòng lặp trong duyệt list ngôn ngữ của người đó.

Đừng lồng quá sâu. Nếu phải lồng nhiều tầng hơn các ví dụ trong bài, nhiều khả năng có cách đơn giản hơn để giải bài toán.

### Dictionary chứa dictionary

Mỗi người dùng trên một website có tên đăng nhập duy nhất, nên tên đăng nhập làm khóa, còn thông tin người dùng là một dictionary:

```python
users = {
    "annguyen": {
        "first": "an",
        "last": "nguyễn",
        "location": "hà nội",
    },
    "binhtran": {
        "first": "bình",
        "last": "trần",
        "location": "đà nẵng",
    },
}

for username, user_info in users.items():
    print(f"\nTên đăng nhập: {username}")
    full_name = f"{user_info['last']} {user_info['first']}"
    location = user_info["location"]

    print(f"\tHọ tên: {full_name.title()}")
    print(f"\tNơi ở: {location.title()}")
```

```text

Tên đăng nhập: annguyen
	Họ tên: Nguyễn An
	Nơi ở: Hà Nội

Tên đăng nhập: binhtran
	Họ tên: Trần Bình
	Nơi ở: Đà Nẵng
```

Các dictionary con có cùng cấu trúc (`first`, `last`, `location`), nên vòng lặp xử lý chúng giống nhau. Python không bắt buộc điều này, nhưng thiếu nó thì code trong vòng lặp sẽ phức tạp hơn nhiều.

## Bài tập

1. **Người quen:** dùng dictionary lưu thông tin một người bạn biết: họ, tên, tuổi, thành phố đang sống. In từng thông tin ra.
2. **Số may mắn:** dùng dictionary lưu số may mắn của năm người, tên người làm khóa. In tên và số may mắn của từng người.
3. **Từ điển lập trình:** chọn năm thuật ngữ bạn đã học trong series (biến, list, vòng lặp, ...) làm khóa, giải thích làm giá trị. Duyệt dictionary và in từng thuật ngữ kèm giải thích, mỗi cặp cách nhau một dòng trống. Thêm năm thuật ngữ nữa, chạy lại để thấy chúng tự xuất hiện trong output.
4. **Sông ngòi:** tạo dictionary gồm ba con sông của Việt Nam và một tỉnh thành mà sông chảy qua, ví dụ `"sông hồng": "hà nội"`. Dùng vòng lặp in một câu cho mỗi sông, ví dụ `Sông Hồng chảy qua Hà Nội.`, rồi in riêng danh sách tên sông và danh sách tỉnh thành.
5. **Nhắc làm khảo sát:** từ dictionary `favorite_languages` trong bài, tạo list những người cần làm khảo sát, gồm cả người đã làm và người chưa làm. Duyệt list: ai đã làm thì in lời cảm ơn, ai chưa làm thì in lời mời.
6. **Danh bạ:** tạo thêm hai dictionary người quen như bài 1, cho cả ba vào list `people`. Duyệt list và in toàn bộ thông tin mỗi người.
7. **Thú cưng:** tạo vài dictionary, mỗi cái là một thú cưng, gồm loài và tên chủ. Cho vào list `pets`, duyệt và in thông tin từng con.
8. **Địa điểm yêu thích:** tạo dictionary `favorite_places`, khóa là tên ba người, giá trị là list từ một đến ba địa điểm người đó thích. Duyệt dictionary và in tên từng người kèm các địa điểm.
9. **Thành phố:** tạo dictionary `cities`, khóa là tên ba tỉnh thành, giá trị là một dictionary gồm miền (Bắc, Trung, Nam), một món đặc sản và một địa điểm du lịch. In tên từng tỉnh thành và các thông tin đó.

## Tóm tắt

- Dictionary lưu các cặp khóa – giá trị trong `{}`. Lấy giá trị bằng `d[key]`, thêm hoặc sửa bằng `d[key] = value`, xóa bằng `del d[key]`.
- `get(key, default)` trả về giá trị mặc định thay vì báo `KeyError` khi khóa không tồn tại.
- Duyệt: `items()` cho cả cặp, `keys()` cho khóa, `values()` cho giá trị. `sorted()` để duyệt theo thứ tự, `set()` để bỏ trùng.
- Có thể lồng list trong dictionary, dictionary trong list, dictionary trong dictionary. Giữ cấu trúc giống nhau và đừng lồng quá sâu.

Bài sau giới thiệu cách nhận dữ liệu người dùng nhập vào và vòng lặp `while`.

## Tài liệu tham khảo

- Eric Matthes, [*Python Crash Course*, 3rd edition](https://nostarch.com/python-crash-course-3rd-edition), chương 6, No Starch Press, 2023.
- [Dictionaries](https://docs.python.org/3/tutorial/datastructures.html#dictionaries) và [Sets](https://docs.python.org/3/tutorial/datastructures.html#sets), Python tutorial.
- [Mapping Types — dict](https://docs.python.org/3/library/stdtypes.html#mapping-types-dict), Python documentation.
