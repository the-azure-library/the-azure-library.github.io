# Giao diện Thư Quán cho Hugo Book

Giải nén vào **thư mục gốc** của repo Hugo (cùng cấp với `hugo.toml`). Không đặt gì vào `themes/`.

```
assets/styles/custom.css                    màu, font, sidebar, khung đọc
layouts/_partials/docs/inject/head.html     nạp font + nhớ chế độ ngày/đêm
layouts/_partials/docs/inject/menu-before.html  nút ngày/đêm trên sidebar
layouts/_partials/docs/inject/footer.html   dòng ghi chú bản quyền cuối trang
layouts/_shortcodes/dau-chuong.html         đầu chương
layouts/_shortcodes/ngat-canh.html          ◆ ◆ ◆
layouts/_shortcodes/ghi-chu.html            ghi chú người dịch
```

## hugo.toml

```toml
[params]
  BookTheme = "thuquan-auto"   # theo máy; nút trên sidebar sẽ ghi đè
  BookToC = false              # tắt mục lục bên phải cho trang chương
  BookSection = "docs"
```

## Một file chương

```markdown
---
title: "Chương I · Thành phố dưới mưa tro"
weight: 1
---
{{< dau-chuong >}}

Đoạn đầu tiên (tự có drop cap).

{{< ngat-canh >}}

{{< ghi-chu >}}
**Ấn ký** là dấu hiệu của người lập khế ước.
{{< /ghi-chu >}}
```

`dau-chuong` tách `title` theo dấu ` · `. Muốn ghi tay thì dùng
`{{< dau-chuong so="Chương I" ten="Tên chương" >}}`.

Thư mục truyện: `content/docs/<slug-truyen>/_index.md` (thêm `bookCollapseSection: true` để thu gọn trên sidebar).
