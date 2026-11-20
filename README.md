# Thư Viện Thanh Không

Site Hugo đăng bản dịch light novel chuyển thể từ game RPG, dùng theme
[Hugo Book](https://github.com/alex-shpak/hugo-book) + bộ giao diện riêng
"Thư Quán" (xem [README-giao-dien.md](README-giao-dien.md)).

## Chạy local

Yêu cầu **Hugo extended >= 0.158**.

```bash
git clone --recurse-submodules https://github.com/the-azure-library/the-azure-library.github.io.git
cd the-azure-library.github.io
hugo server
```

Nếu đã clone mà quên `--recurse-submodules`:

```bash
git submodule update --init --recursive
```

## Thêm một truyện mới

1. Tạo thư mục `content/docs/<slug-truyen>/`.
2. Tạo `content/docs/<slug-truyen>/_index.md`:

   ```yaml
   ---
   title: "Tên Truyện"
   weight: 2            # số thứ tự hiển thị trên sidebar
   bookCollapseSection: true
   ---

   **Game gốc:** ...
   **Tác giả gốc:** ...
   **Link nguồn:** ...
   ```

## Thêm một chương

Tạo `content/docs/<slug-truyen>/chuong-XX.md`:

```yaml
---
title: "Chương I · Tên chương"   # dau-chuong tách theo dấu " · "
weight: 1
---
{{< dau-chuong >}}

Đoạn văn đầu tiên...

{{< ngat-canh >}}

Đoạn văn sau cảnh chuyển...
```

`title` nên viết đúng dạng `Chương <số> · <tên chương>` vì shortcode
`dau-chuong` tự tách chữ số và tên chương từ đó. Muốn ghi tay số/tên khác
thì dùng `{{< dau-chuong so="Chương I" ten="Tên chương" >}}`.

## Các shortcode có sẵn

| Shortcode | Dùng khi | Cách gọi |
|---|---|---|
| `dau-chuong` | Dòng đầu tiên của mỗi chương | `{{< dau-chuong >}}` |
| `ngat-canh` | Ngắt cảnh giữa hai đoạn (hiện `◆ ◆ ◆`) | `{{< ngat-canh >}}` |
| `ghi-chu` | Chú thích/giải nghĩa của người dịch | `{{< ghi-chu >}}Nội dung chú thích.{{< /ghi-chu >}}` |

Xem truyện đầu tiên **Alice Mare**
(`content/docs/alice-mare/`) để có ví dụ đầy đủ.

## Thêm trang / bài blog

- Trang đơn (như "Giới thiệu"): thêm file `.md` trực tiếp trong `content/`,
  rồi khai báo đường dẫn trong `[menu.after]` ở `hugo.toml` nếu muốn hiện
  trên sidebar.
- Bài blog: thêm file trong `content/blog/`, ví dụ
  `content/blog/ten-bai-viet.md` với front matter có `title`, `date`.

## Nút chương trước/sau

`layouts/_partials/docs/prev-next.html` đã được ghi đè so với theme gốc:
nút "chương trước/sau" chỉ di chuyển trong **cùng một truyện** (cùng thư
mục cha dưới `content/docs/`), không nhảy qua truyện khác khi hết chương
đầu/cuối. Nếu thêm truyện mới theo đúng cấu trúc thư mục ở trên, không
cần sửa gì thêm ở file này.

## Build & kiểm tra trước khi push

```bash
hugo                 # build ra ./public, báo lỗi nếu có
hugo server          # xem thử tại http://localhost:1313
```

Kiểm tra nhanh trên một trang chương: đầu chương (số + tên + hoạ tiết),
chữ cái đầu đoạn (drop cap), dấu ngắt cảnh `◆ ◆ ◆`, khung ghi chú người
dịch, nút đổi ngày/đêm trên sidebar, và nút chương trước/sau ở cuối trang.

## Deploy lên GitHub Pages

Deploy tự động qua `.github/workflows/hugo.yml` mỗi khi push lên nhánh
`main` (dùng Hugo extended phiên bản ghi trong `HUGO_VERSION` của file
workflow — cập nhật số này khi muốn nâng Hugo).

```bash
git add -A
git commit -m "Nội dung: ..."
git push origin main
```

Sau khi Actions chạy xong (tab **Actions** trên GitHub), site sẽ có tại
`https://the-azure-library.github.io/`.

### Cần tự bấm trên GitHub (một lần)

1. **Settings → Pages → Build and deployment → Source** = chọn
   **GitHub Actions** (không chọn "Deploy from a branch").
2. Nếu repo đang **Private**: GitHub Pages cho repo private chỉ hoạt
   động với GitHub Pro/Team/Enterprise, hoặc đổi repo sang **Public**
   ở **Settings → General → Danger Zone → Change visibility**.
3. (Tuỳ chọn) **Settings → Actions → General → Workflow permissions**:
   đảm bảo Actions được phép chạy (mặc định đã bật với workflow dùng
   `permissions: pages: write`, không cần đổi thêm gì nếu không bị tổ
   chức giới hạn).
4. Sau lần push đầu tiên, vào tab **Actions** xem job `Deploy Hugo site
   to GitHub Pages` chạy xanh; nếu đỏ, mở log để xem lỗi build.

## Cấu trúc thư mục chính

```
content/
  _index.md              trang chủ (layout: landing)
  gioi-thieu.md          trang Giới thiệu
  blog/                  mục Blog
  docs/
    _index.md            trang gốc mục "Thư viện"
    <slug-truyen>/
      _index.md          giới thiệu truyện
      chuong-01.md, ...   các chương
layouts/
  _partials/docs/prev-next.html   override chương trước/sau theo truyện
  _partials/docs/inject/          hook head/menu/footer của bộ giao diện
  _shortcodes/                    dau-chuong, ngat-canh, ghi-chu
assets/styles/custom.css          CSS của bộ giao diện Thư Quán
themes/hugo-book/                 theme gốc (submodule, không sửa)
```
