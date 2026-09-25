# nguyencongthanh.io.vn

Site tĩnh xây dựng bằng [Hugo](https://gohugo.io/) (extended v0.166.0) với theme [LoveIt](https://github.com/dillonzq/LoveIt), triển khai lên GitHub Pages.

## Clone

Theme được quản lý bằng git submodule:

```sh
git clone --recurse-submodules git@github.com:nguyen-cong-thanh/nguyencongthanh.io.vn.git
# hoặc, với bản clone đã có:
git submodule update --init --recursive
```

## Chạy local

Chỉ cần Docker. Lệnh sau chạy `hugo server` (bao gồm bài nháp) tại http://localhost:1313/:

```sh
docker compose up
```

Container chạy với UID/GID `1000:1000` để file sinh ra thuộc user hiện tại. Nếu UID/GID máy khác, export biến `UID`/`GID` trước khi chạy.

Build một lần ra thư mục `public/`:

```sh
docker compose run --rm hugo --gc --minify
```

## Viết bài song ngữ

Ngôn ngữ mặc định là tiếng Việt (URL gốc `/`), tiếng Anh nằm dưới `/en/`. Mỗi bài là một thư mục, mỗi ngôn ngữ một file; hai file cùng thư mục được Hugo liên kết là bản dịch của nhau:

```
content/posts/<slug>/index.vi.md
content/posts/<slug>/index.en.md
```

Tạo bài mới:

```sh
docker compose run --rm hugo new content posts/<slug>/index.vi.md
docker compose run --rm hugo new content posts/<slug>/index.en.md
```

Bài mới có `draft = true`; đổi thành `false` để xuất bản.

## Triển khai

Workflow [.github/workflows/hugo.yaml](.github/workflows/hugo.yaml) build và deploy khi push lên `main`. Phiên bản Hugo khai báo ở `HUGO_VERSION` trong workflow và ở tag image trong [compose.yaml](compose.yaml); khi nâng cấp cần đổi cả hai.

Thiết lập một lần:

1. GitHub → Settings → Pages → Source: **GitHub Actions**. Custom domain: `nguyencongthanh.io.vn`, bật Enforce HTTPS sau khi chứng chỉ được cấp.
2. DNS trên Cloudflare cho apex `nguyencongthanh.io.vn` (không ảnh hưởng các subdomain dùng cloudflared tunnel):
   - 4 bản ghi A: `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`; hoặc một CNAME `@` → `nguyen-cong-thanh.github.io`.
   - Để chế độ **DNS only** cho tới khi GitHub cấp xong chứng chỉ. Nếu sau đó bật proxy thì đặt SSL/TLS mode là **Full (strict)**.
3. Tùy chọn: xác minh domain ở phần Pages trong cài đặt tài khoản GitHub (bản ghi TXT do GitHub cung cấp).

File `static/CNAME` giữ custom domain trong mỗi lần deploy.
