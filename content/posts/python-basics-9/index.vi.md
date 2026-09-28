+++
title = "Python cơ bản – Bài 9: Class"
date = 2026-09-25T09:09:00+07:00
draft = false
tags = ["python", "lập trình cơ bản"]
series = ["Python cơ bản"]
series_weight = 9
+++

Ở bài 6, mình dùng dictionary để lưu thông tin một món đồ uống, và ở bài 8 dùng hàm để xử lý dữ liệu. Nhưng dữ liệu và hàm xử lý nó vẫn nằm tách rời nhau. *Class* gom cả hai vào một chỗ: một chiếc xe vừa có hãng, mẫu, số km đã chạy, vừa có các hành động như cập nhật số km. Đây là nền tảng của *lập trình hướng đối tượng* (object-oriented programming, OOP), cách tổ chức code mà bạn sẽ gặp ở hầu hết các thư viện Python. Bài này giới thiệu cách viết class, tạo đối tượng, kế thừa, và import class từ module.

<!--more-->

## Bài này dành cho ai

Người mới bắt đầu, đã đọc [bài 8](../python-basics-8/) về hàm và module. Cần Python 3.14.

## Tạo và dùng class

Một class mô tả *một loại* đối tượng, không phải một đối tượng cụ thể. Class `Dog` mô tả con chó nói chung: con nào cũng có tên, tuổi, và biết ngồi, biết lăn. Từ class đó, bạn tạo ra từng con chó cụ thể. Việc tạo một đối tượng từ class gọi là *khởi tạo* (instantiation), và đối tượng tạo ra gọi là *instance*.

### Class Dog

Lưu class này vào file `dog.py`:

<!-- file: dog.py -->
```python
class Dog:
    """Mô phỏng đơn giản một con chó."""

    def __init__(self, name, age):
        """Khởi tạo thuộc tính tên và tuổi."""
        self.name = name
        self.age = age

    def sit(self):
        """Mô phỏng con chó ngồi xuống khi được ra lệnh."""
        print(f"{self.name} đã ngồi xuống.")

    def roll_over(self):
        """Mô phỏng con chó lăn một vòng khi được ra lệnh."""
        print(f"{self.name} đã lăn một vòng!")
```

- Theo quy ước, tên class viết hoa chữ cái đầu mỗi từ: `Dog`, `ElectricCar`.
- Hàm nằm trong class gọi là *method*. Mọi điều bạn biết về hàm đều áp dụng cho method.

### Method `__init__()`

`__init__()` là method đặc biệt mà Python tự chạy mỗi khi bạn tạo một instance mới. Hai dấu gạch dưới ở mỗi bên là quy ước để method đặc biệt không trùng tên với method thường.

- Tham số `self` là bắt buộc và phải đứng đầu. Nó là tham chiếu tới chính instance đang được tạo, giúp instance truy cập được thuộc tính và method của nó. Khi gọi, bạn không truyền `self`, Python tự truyền.
- `self.name = name` lấy giá trị của tham số `name` và gắn vào instance. Biến có tiền tố `self.` gọi là *thuộc tính* (attribute), dùng được ở mọi method trong class và qua mọi instance.

### Tạo instance

Class như một bản hướng dẫn cách tạo instance. Các ví dụ dưới đây import class `Dog` từ `dog.py` bằng `from dog import Dog`, cách import class được giải thích ở cuối bài. Bạn cũng có thể chép class vào ngay đầu file. Tạo một con chó cụ thể:

```python
from dog import Dog

my_dog = Dog("Vàng", 6)

print(f"Chó nhà mình tên {my_dog.name}.")
print(f"Nó {my_dog.age} tuổi.")
```

```text
Chó nhà mình tên Vàng.
Nó 6 tuổi.
```

`Dog("Vàng", 6)` gọi `__init__()` với `name="Vàng"` và `age=6`, rồi trả về instance vừa tạo, được gán vào biến `my_dog`. Quy ước: tên class viết hoa (`Dog`), tên instance viết thường (`my_dog`).

### Truy cập thuộc tính và gọi method

Dùng dấu chấm: `my_dog.name` là thuộc tính `name` của `my_dog`, chính là `self.name` bên trong class. Gọi method cũng vậy:

```python
from dog import Dog

my_dog = Dog("Vàng", 6)
my_dog.sit()
my_dog.roll_over()
```

```text
Vàng đã ngồi xuống.
Vàng đã lăn một vòng!
```

Khi thuộc tính và method có tên rõ nghĩa như `name`, `sit()`, bạn đoán được đoạn code làm gì ngay cả khi chưa đọc class.

### Tạo nhiều instance

```python
from dog import Dog

my_dog = Dog("Vàng", 6)
your_dog = Dog("Mực", 3)

print(f"Chó nhà mình tên {my_dog.name}, {my_dog.age} tuổi.")
my_dog.sit()

print(f"\nChó nhà bạn tên {your_dog.name}, {your_dog.age} tuổi.")
your_dog.sit()
```

```text
Chó nhà mình tên Vàng, 6 tuổi.
Vàng đã ngồi xuống.

Chó nhà bạn tên Mực, 3 tuổi.
Mực đã ngồi xuống.
```

Mỗi instance có bộ thuộc tính riêng. Kể cả khi hai con chó trùng tên, trùng tuổi, Python vẫn tạo hai instance riêng biệt.

## Làm việc với class và instance

### Class Car

```python
class Car:
    """Mô phỏng đơn giản một chiếc xe ô tô."""

    def __init__(self, make, model, year):
        """Khởi tạo các thuộc tính mô tả xe."""
        self.make = make
        self.model = model
        self.year = year

    def get_descriptive_name(self):
        """Trả về tên mô tả của xe."""
        long_name = f"{self.make} {self.model} {self.year}"
        return long_name.title()

my_new_car = Car("toyota", "vios", 2024)
print(my_new_car.get_descriptive_name())
```

```text
Toyota Vios 2024
```

### Thuộc tính có giá trị mặc định

Thuộc tính không nhất thiết phải truyền vào qua tham số. Thêm thuộc tính `odometer_reading` (số km trên đồng hồ công-tơ-mét), luôn bắt đầu từ 0, và method để đọc nó:

<!-- file: car.py -->
```python
"""Class dùng để mô phỏng xe ô tô."""

class Car:
    """Mô phỏng đơn giản một chiếc xe ô tô."""

    def __init__(self, make, model, year):
        """Khởi tạo các thuộc tính mô tả xe."""
        self.make = make
        self.model = model
        self.year = year
        self.odometer_reading = 0

    def get_descriptive_name(self):
        """Trả về tên mô tả của xe."""
        long_name = f"{self.make} {self.model} {self.year}"
        return long_name.title()

    def read_odometer(self):
        """In số km xe đã chạy."""
        print(f"Xe đã chạy {self.odometer_reading} km.")

    def update_odometer(self, mileage):
        """
        Đặt số km trên đồng hồ.
        Từ chối nếu số mới nhỏ hơn số hiện tại.
        """
        if mileage >= self.odometer_reading:
            self.odometer_reading = mileage
        else:
            print("Không được tua ngược đồng hồ công-tơ-mét!")

    def increment_odometer(self, km):
        """Cộng thêm số km vào đồng hồ."""
        self.odometer_reading += km
```

Đây là class `Car` đầy đủ, gồm cả các method ở các mục sau. Mình lưu nó trong file `car.py` và dòng đầu tiên là *docstring của module*, mô tả module chứa gì. Các ví dụ tiếp theo import class từ file này.

```python
from car import Car

my_new_car = Car("toyota", "vios", 2024)
print(my_new_car.get_descriptive_name())
my_new_car.read_odometer()
```

```text
Toyota Vios 2024
Xe đã chạy 0 km.
```

### Sửa giá trị thuộc tính

Có ba cách: sửa trực tiếp qua instance, đặt giá trị qua method, và cộng dồn qua method.

**Sửa trực tiếp:**

```python
from car import Car

my_new_car = Car("toyota", "vios", 2024)
my_new_car.odometer_reading = 23
my_new_car.read_odometer()
```

```text
Xe đã chạy 23 km.
```

**Qua method:** `update_odometer()` nhận giá trị mới và tự cập nhật thuộc tính. Lợi ích là method có thể kiểm tra dữ liệu trước khi sửa, ở đây là không cho tua ngược số km:

```python
from car import Car

my_new_car = Car("toyota", "vios", 2024)
my_new_car.update_odometer(23)
my_new_car.read_odometer()

my_new_car.update_odometer(10)
my_new_car.read_odometer()
```

```text
Xe đã chạy 23 km.
Không được tua ngược đồng hồ công-tơ-mét!
Xe đã chạy 23 km.
```

**Cộng dồn qua method:** mua một chiếc xe cũ, rồi chạy thêm 100 km trước khi đăng ký:

```python
from car import Car

my_used_car = Car("honda", "city", 2019)
print(my_used_car.get_descriptive_name())

my_used_car.update_odometer(23_500)
my_used_car.read_odometer()

my_used_car.increment_odometer(100)
my_used_car.read_odometer()
```

```text
Honda City 2019
Xe đã chạy 23500 km.
Xe đã chạy 23600 km.
```

Method giúp kiểm soát cách sửa dữ liệu, nhưng không ngăn được ai đó sửa thẳng thuộc tính như cách đầu tiên. Bảo mật thật sự cần nhiều hơn những kiểm tra đơn giản này.

## Kế thừa

Nếu class bạn định viết là một phiên bản chuyên biệt của class đã có, bạn có thể dùng *kế thừa* (inheritance). Class con nhận toàn bộ thuộc tính và method của class cha, và có thể thêm thuộc tính, method của riêng nó.

### `__init__()` của class con

Xe điện là một loại xe ô tô, nên class `ElectricCar` kế thừa từ `Car`:

```python
from car import Car

class ElectricCar(Car):
    """Mô phỏng những điểm riêng của xe điện."""

    def __init__(self, make, model, year):
        """Khởi tạo các thuộc tính của class cha."""
        super().__init__(make, model, year)

my_ev = ElectricCar("vinfast", "vf 8", 2024)
print(my_ev.get_descriptive_name())
```

```text
Vinfast Vf 8 2024
```

- Tên class cha đặt trong ngoặc đơn khi định nghĩa class con: `class ElectricCar(Car):`. Class cha phải có sẵn trước, ở đây được import từ `car.py`.
- `super()` trả về class cha. `super().__init__(...)` gọi `__init__()` của `Car`, để instance của `ElectricCar` có đủ các thuộc tính của `Car`.
- Tên xe hiện thành `Vinfast Vf 8` vì `title()` viết hoa chữ đầu của mỗi từ và viết thường phần còn lại. Muốn giữ đúng `VinFast VF 8`, hãy truyền tên đã viết đúng và bỏ `title()`.

### Thêm thuộc tính và method cho class con

```python
from car import Car

class ElectricCar(Car):
    """Mô phỏng những điểm riêng của xe điện."""

    def __init__(self, make, model, year):
        """
        Khởi tạo các thuộc tính của class cha,
        rồi khởi tạo thuộc tính riêng của xe điện.
        """
        super().__init__(make, model, year)
        self.battery_size = 40

    def describe_battery(self):
        """In dung lượng pin."""
        print(f"Xe có pin {self.battery_size} kWh.")

my_ev = ElectricCar("vinfast", "vf 8", 2024)
my_ev.describe_battery()
```

```text
Xe có pin 40 kWh.
```

`battery_size` chỉ có ở instance của `ElectricCar`, không có ở `Car`. Thuộc tính hay method nào đúng với mọi xe thì nên đặt ở `Car`, chỉ những gì riêng của xe điện mới đặt ở `ElectricCar`.

### Ghi đè method của class cha

Method nào của class cha không phù hợp với class con, bạn định nghĩa lại method cùng tên trong class con. Python sẽ dùng method của class con. Giả sử `Car` có method `fill_gas_tank()` để đổ xăng:

<!-- norun -->
```python
class ElectricCar(Car):
    # ...

    def fill_gas_tank(self):
        """Xe điện không có bình xăng."""
        print("Xe này không có bình xăng!")
```

### Dùng instance làm thuộc tính

Class có nhiều chi tiết thì dài ra rất nhanh. Khi đó, tách một phần thành class riêng. Ví dụ pin có thể là một class `Battery`, và `ElectricCar` có một thuộc tính là instance của `Battery`:

```python
from car import Car

class Battery:
    """Mô phỏng đơn giản pin của xe điện."""

    def __init__(self, battery_size=40):
        """Khởi tạo thuộc tính của pin."""
        self.battery_size = battery_size

    def describe_battery(self):
        """In dung lượng pin."""
        print(f"Xe có pin {self.battery_size} kWh.")

    def get_range(self):
        """In quãng đường đi được khi sạc đầy."""
        if self.battery_size == 40:
            range_km = 250
        elif self.battery_size == 65:
            range_km = 400
        print(f"Sạc đầy đi được khoảng {range_km} km.")

class ElectricCar(Car):
    """Mô phỏng những điểm riêng của xe điện."""

    def __init__(self, make, model, year):
        """
        Khởi tạo các thuộc tính của class cha,
        rồi khởi tạo thuộc tính riêng của xe điện.
        """
        super().__init__(make, model, year)
        self.battery = Battery()

my_ev = ElectricCar("vinfast", "vf 8", 2024)
print(my_ev.get_descriptive_name())
my_ev.battery.describe_battery()
my_ev.battery.get_range()
```

```text
Vinfast Vf 8 2024
Xe có pin 40 kWh.
Sạc đầy đi được khoảng 250 km.
```

Các con số dung lượng pin và quãng đường ở đây chỉ để minh họa, không phải thông số của mẫu xe thật nào.

`my_ev.battery.get_range()` đọc là: tìm thuộc tính `battery` của `my_ev`, rồi gọi method `get_range()` của instance `Battery` đó. Giờ bạn có thể mô tả pin chi tiết bao nhiêu tùy ý mà class `ElectricCar` vẫn gọn.

Để ý mình đặt tên biến là `range_km` chứ không phải `range`, vì `range` là tên một hàm có sẵn của Python và đặt biến trùng tên sẽ che mất hàm đó.

## Import class

Class nhiều lên thì file dài ra. Giống hàm, class có thể lưu trong module rồi import vào chương trình chính. Bạn đã thấy cách này ở trên: class `Car` nằm trong `car.py`, và các ví dụ dùng `from car import Car`.

### Nhiều class trong một module

Một module có thể chứa nhiều class, miễn là chúng liên quan tới nhau. `Battery` và `ElectricCar` đều dùng để mô phỏng xe, nên có thể đặt chung. Tạo file `electric_car.py`:

<!-- file: electric_car.py -->
```python
"""Các class dùng để mô phỏng xe điện."""

from car import Car

class Battery:
    """Mô phỏng đơn giản pin của xe điện."""

    def __init__(self, battery_size=40):
        """Khởi tạo thuộc tính của pin."""
        self.battery_size = battery_size

    def describe_battery(self):
        """In dung lượng pin."""
        print(f"Xe có pin {self.battery_size} kWh.")

class ElectricCar(Car):
    """Mô phỏng những điểm riêng của xe điện."""

    def __init__(self, make, model, year):
        """Khởi tạo thuộc tính của class cha và pin."""
        super().__init__(make, model, year)
        self.battery = Battery()
```

`ElectricCar` cần class cha `Car`, nên module `electric_car.py` import `Car` từ `car.py`. Một module import từ module khác là chuyện bình thường.

### Các cách import class

Cách import class giống hệt cách import hàm ở bài 8:

```python
from car import Car
from electric_car import ElectricCar as EC
import electric_car

my_car = Car("toyota", "vios", 2024)
print(my_car.get_descriptive_name())

my_ev = EC("vinfast", "vf 8", 2024)
print(my_ev.get_descriptive_name())

my_other_ev = electric_car.ElectricCar("vinfast", "vf 5", 2024)
my_other_ev.battery.describe_battery()
```

```text
Toyota Vios 2024
Vinfast Vf 8 2024
Xe có pin 40 kWh.
```

- `from car import Car`: import một class. Import nhiều class thì cách nhau bằng dấu phẩy: `from electric_car import Battery, ElectricCar`.
- `as EC`: đặt bí danh cho class.
- `import electric_car`: import cả module, dùng class qua `electric_car.ElectricCar`. Cách này không bao giờ bị trùng tên với code trong file hiện tại.

Tránh `from module import *`, vì nhìn các dòng import ở đầu file bạn sẽ không biết chương trình dùng những class nào.

Khi mới bắt đầu, cứ viết mọi thứ trong một file. Chạy ổn rồi mới tách class ra module.

## Thư viện chuẩn của Python

*Thư viện chuẩn* (standard library) là bộ module có sẵn khi cài Python. Giờ bạn đã hiểu hàm và class, bạn có thể dùng module do người khác viết. Ví dụ module `random`:

<!-- norun -->
```python
from random import randint, choice

print(randint(1, 6))

players = ["an", "bình", "cường", "dũng", "đức"]
print(choice(players))
```

`randint(1, 6)` trả về một số nguyên ngẫu nhiên từ 1 đến 6, tính cả hai đầu. `choice()` chọn ngẫu nhiên một phần tử trong list hoặc tuple. Mỗi lần chạy sẽ ra kết quả khác nhau. Đừng dùng `random` cho những việc liên quan tới bảo mật, như tạo mật khẩu, còn lại thì dùng tốt.

Ngoài thư viện chuẩn, bạn còn cài được module của bên thứ ba bằng `pip`. Các bài nâng cao trên blog sẽ dùng nhiều module như vậy.

## Trình bày class

- Tên class viết kiểu *CamelCase*: viết hoa chữ cái đầu mỗi từ, không dùng dấu gạch dưới (`ElectricCar`). Tên instance và tên module viết thường, nối từ bằng dấu gạch dưới (`my_car`, `electric_car.py`).
- Mỗi class có docstring ngay dưới dòng `class`, mỗi module có docstring ở đầu file.
- Trong class, để một dòng trống giữa các method. Trong module, để hai dòng trống giữa các class.
- Import module của thư viện chuẩn trước, dòng trống, rồi mới import module bạn tự viết.

## Bài tập

1. **Quán ăn:** viết class `Restaurant` có thuộc tính `restaurant_name` và `cuisine_type` (loại món), method `describe_restaurant()` in hai thông tin đó, và `open_restaurant()` in thông báo quán đang mở cửa. Tạo ba instance, ví dụ quán phở, quán bún chả, quán cơm tấm, và gọi `describe_restaurant()` cho từng quán.
2. **Người dùng:** viết class `User` có thuộc tính `first_name`, `last_name` và vài thuộc tính khác. Viết method `describe_user()` in thông tin người dùng và `greet_user()` in lời chào. Tạo vài người dùng và gọi cả hai method.
3. **Số khách:** thêm thuộc tính `number_served` mặc định bằng 0 vào `Restaurant`. In số khách, sửa trực tiếp rồi in lại. Viết thêm `set_number_served()` để đặt số khách và `increment_number_served()` để cộng thêm khách trong ngày.
4. **Đăng nhập sai:** thêm thuộc tính `login_attempts` vào `User`, method `increment_login_attempts()` tăng nó lên 1 và `reset_login_attempts()` đặt lại về 0. Gọi `increment_login_attempts()` vài lần, in giá trị, rồi gọi `reset_login_attempts()` và in lại.
5. **Quán kem:** viết class `IceCreamStand` kế thừa `Restaurant`, thêm thuộc tính `flavors` là list các vị kem (dừa, sầu riêng, cốm, ...) và method in các vị kem.
6. **Admin:** viết class `Admin` kế thừa `User`, thêm thuộc tính `privileges` là list quyền (`"xóa bài"`, `"khóa tài khoản"`, ...) và method `show_privileges()`. Sau đó tách quyền ra class riêng `Privileges` và dùng instance của nó làm thuộc tính của `Admin`.
7. **Nâng cấp pin:** thêm method `upgrade_battery()` vào `Battery`, đổi dung lượng pin thành 65 nếu chưa phải. Tạo một xe điện, gọi `get_range()`, nâng cấp pin, rồi gọi `get_range()` lần nữa.
8. **Tách module:** lưu `Restaurant` vào một module và import vào chương trình khác. Làm tương tự với `User`, `Privileges`, `Admin`: thử để cả ba trong một module, rồi tách `User` sang module riêng.
9. **Xúc xắc:** viết class `Die` có thuộc tính `sides` mặc định là 6, method `roll_die()` in một số ngẫu nhiên từ 1 đến `sides`. Tung xúc xắc 6 mặt 10 lần, rồi làm tương tự với xúc xắc 10 mặt và 20 mặt.
10. **Vé số:** tạo list gồm 10 chữ số và 5 chữ cái. Chọn ngẫu nhiên 4 phần tử làm dãy trúng thưởng và in ra thông báo: vé nào trùng dãy này thì trúng. Sau đó viết vòng lặp tạo vé ngẫu nhiên cho tới khi trúng, và in số lần phải mua vé.

## Tóm tắt

- Class mô tả một loại đối tượng. Instance là một đối tượng cụ thể tạo từ class.
- `__init__(self, ...)` chạy khi tạo instance. `self.ten = gia_tri` tạo thuộc tính. Method là hàm trong class, luôn có `self` là tham số đầu tiên.
- Truy cập thuộc tính và gọi method bằng dấu chấm: `my_car.model`, `my_car.read_odometer()`.
- Sửa thuộc tính trực tiếp hoặc qua method. Method cho phép kiểm tra dữ liệu trước khi sửa.
- Kế thừa: `class Con(Cha):`, gọi `super().__init__(...)` trong `__init__()` của class con. Class con có thể thêm hoặc ghi đè method.
- Một class có thể dùng instance của class khác làm thuộc tính.
- Class lưu trong module và import giống hàm. Thư viện chuẩn có sẵn nhiều module như `random`.

Bài sau, cũng là bài cuối của series, giới thiệu cách đọc ghi file và xử lý lỗi bằng exception.

## Tài liệu tham khảo

- Eric Matthes, [*Python Crash Course*, 3rd edition](https://nostarch.com/python-crash-course-3rd-edition), chương 9, No Starch Press, 2023.
- [Classes](https://docs.python.org/3/tutorial/classes.html), Python tutorial.
- [random — Generate pseudo-random numbers](https://docs.python.org/3/library/random.html), Python documentation.
- [PEP 8 – Naming Conventions](https://peps.python.org/pep-0008/#naming-conventions)
