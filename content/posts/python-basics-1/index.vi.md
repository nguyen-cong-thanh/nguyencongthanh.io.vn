+++
title = "Python cơ bản – Bài 1: Giới thiệu series"
date = 2026-09-25
draft = false
tags = ["python", "lập trình cơ bản"]
series = ["Python cơ bản"]
series_weight = 1
+++

<!-- TODO: câu chuyện cá nhân — vì sao bạn viết series này, ví dụ lần đầu bạn học lập trình hoặc câu hỏi anh em hay hỏi bạn. -->

Nhiều bài trên blog này dùng Python để minh họa: gọi API, xử lý dữ liệu, viết script tự động hóa. Nếu bạn chưa từng viết một dòng code nào, đọc những bài đó sẽ khá vất vả. Series "Python cơ bản" viết ra để lấp khoảng trống đó. Đọc hết series, bạn sẽ nắm được những khái niệm lập trình nền tảng và đủ sức theo các bài nâng cao hơn.

<!--more-->

## Series này dựa trên sách nào

Series đi theo trình tự các chủ đề trong Phần I của cuốn [*Python Crash Course*, 3rd edition](https://nostarch.com/python-crash-course-3rd-edition) của Eric Matthes (No Starch Press, 2023). Sách đi từ biến và chuỗi cho tới class và file, kèm nhiều bài tập nhỏ.

Mình không dịch nguyên văn. Nội dung được viết lại theo cách của mình cho dễ đọc, một số ví dụ và bài tập được sửa lại cho gần gũi với anh em Việt Nam: tên người Việt, món ăn Việt, địa danh Việt Nam. Một vài phần mình thấy không cần thiết với người mới thì được lược bớt. Nếu bạn muốn đọc đầy đủ, có thêm Phần II với ba project (game, trực quan hóa dữ liệu, web app), mình khuyên bạn mua sách.

## Series dành cho ai

- Người chưa biết lập trình, hoặc mới biết một chút và muốn học lại cho có hệ thống.
- Người đã biết một ngôn ngữ khác và muốn làm quen nhanh với cú pháp Python. Bạn có thể đọc lướt và chỉ làm phần bài tập.

Bạn cần một máy tính (Windows, macOS hoặc Linux) và biết mở terminal. Không cần kiến thức toán hay lập trình trước đó.

## Chuẩn bị môi trường

Toàn bộ code trong series chạy trên **Python 3.14**.

1. Cài Python từ [python.org/downloads](https://www.python.org/downloads/). Trên Windows, nhớ tick ô "Add python.exe to PATH" ở màn hình cài đặt đầu tiên.
2. Cài một editor, ví dụ [VS Code](https://code.visualstudio.com/) kèm extension Python của Microsoft. <!-- TODO: editor bạn dùng, nếu muốn nhắc tới. -->
3. Mở terminal và kiểm tra phiên bản:

```bash
python --version
```

```text
Python 3.14.7
```

Trên macOS và Linux, lệnh có thể là `python3` thay vì `python`. Nếu gặp trục trặc, tác giả sách có [hướng dẫn cài đặt chi tiết](https://ehmatthes.github.io/pcc_3e/setup_instructions/) cho từng hệ điều hành.

Để chạy một file Python, mở terminal ở thư mục chứa file rồi gõ:

```bash
python xin_chao.py
```

## Nội dung series

1. Giới thiệu series (bài này)
2. Biến và kiểu dữ liệu đơn giản: chuỗi, số, comment
3. Làm quen với list
4. Làm việc với list: vòng lặp `for`, slice, tuple
5. Câu lệnh `if`
6. Dictionary
7. Nhập dữ liệu với `input()` và vòng lặp `while`
8. Hàm
9. Class
10. File và exception

Mỗi bài có một mục bài tập ở cuối. Bạn nên tự gõ lại code và làm bài tập thay vì chỉ đọc, vì phần lớn kỹ năng lập trình đến từ việc gõ, chạy và sửa lỗi.

## Tóm tắt

- Series đi theo Phần I của *Python Crash Course*, 3rd edition, được viết lại bằng tiếng Việt với ví dụ Việt hóa.
- Bạn cần Python 3.14 và một editor.
- Chạy file bằng lệnh `python ten_file.py` trong terminal.

## Tài liệu tham khảo

- Eric Matthes, [*Python Crash Course*, 3rd edition](https://nostarch.com/python-crash-course-3rd-edition), No Starch Press, 2023.
- [Tài liệu bổ trợ của sách](https://ehmatthes.github.io/pcc_3e/): hướng dẫn cài đặt, cheat sheet, lời giải bài tập.
- [Python downloads](https://www.python.org/downloads/)
