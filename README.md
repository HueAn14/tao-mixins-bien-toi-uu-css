# [Thực hành] Tạo mixins & biến để tối ưu CSS của website mẫu

> **Khóa học:** CodeGym Web Development  
> **Học viên:** HueAn14  
> **Repository:** [https://github.com/HueAn14/tao-mixins-bien-toi-uu-css](https://github.com/HueAn14/tao-mixins-bien-toi-uu-css)

---

## 🎯 Mục tiêu
- **Tạo mixins** giúp đóng gói mã CSS dùng chung, giảm lặp code đáng kể.
- **Dùng biến (variables)** để kiểm soát các giá trị giao diện (màu sắc, cỡ chữ, khoảng đệm, bóng mờ) tại một nơi duy nhất.
- **Cải thiện tính tái sử dụng và khả năng bảo trì** của mã nguồn CSS.

---

## 📁 Cấu trúc thư mục

```
tao-mixins-bien-toi-uu-css/
├── index.html                  # Trang demo giao diện trực quan
├── styles-original.css         # Bước 1: Mã nguồn CSS gốc trước tối ưu
├── abstracts/
│   ├── _variables.scss         # Bước 2: Khai báo biến SASS
│   └── _mixins.scss            # Bước 3: Khai báo Mixins tái sử dụng
├── base/
│   └── _typography.scss        # Bước 4: Áp dụng biến vào h1, h2
├── components/
│   ├── _buttons.scss           # Bước 4: Áp dụng button-style mixin
│   └── _cards.scss             # Bước 4: Áp dụng box-style mixin
├── main.scss                   # File chính tổng hợp (import) toàn bộ partials
├── main.css                    # File CSS sau khi biên dịch từ main.scss
├── package.json                # Cấu hình npm scripts biên dịch SASS
└── README.md                   # Tài liệu hướng dẫn & giải thích
```

---

## 📌 Các bước thực hiện

### Bước 1: File CSS trước khi tối ưu (`styles-original.css`)
```css
h1 {
  font-size: 32px;
  color: #222;
}

h2 {
  font-size: 28px;
  color: #222;
}

.button {
  background: red;
  color: white;
  padding: 10px 20px;
  border-radius: 5px;
}

.card {
  background: white;
  padding: 15px;
  border-radius: 10px;
  box-shadow: 2px 2px 10px rgba(0, 0, 0, 0.1);
}
```

### Bước 2: Tạo biến trong SASS (`abstracts/_variables.scss`)
```scss
$primary-color: red;
$text-color: #222;
$font-large: 32px;
$font-medium: 28px;
$padding-standard: 10px 20px;
$box-shadow-default: 2px 2px 10px rgba(0, 0, 0, 0.1);
```

### Bước 3: Tạo Mixins tái sử dụng (`abstracts/_mixins.scss`)
```scss
@mixin button-style($bg-color) {
  background: $bg-color;
  color: white;
  padding: $padding-standard;
  border-radius: 5px;
}

@mixin box-style {
  background: white;
  padding: 15px;
  border-radius: 10px;
  box-shadow: $box-shadow-default;
}
```

### Bước 4: Áp dụng Mixins & Biến vào SCSS
- **`base/_typography.scss`:**
```scss
h1 {
  font-size: $font-large;
  color: $text-color;
}

h2 {
  font-size: $font-medium;
  color: $text-color;
}
```

- **`components/_buttons.scss`:**
```scss
.button {
  @include button-style($primary-color);
}
```

- **`components/_cards.scss`:**
```scss
.card {
  @include box-style;
}
```

- **`main.scss`:**
```scss
@import 'abstracts/variables';
@import 'abstracts/mixins';
@import 'base/typography';
@import 'components/buttons';
@import 'components/cards';
```

---

## 📊 Bảng so sánh trước và sau tối ưu

| Tiêu chí | Trước tối ưu (CSS thuần) | Sau tối ưu (SASS Variables & Mixins) |
|---|---|---|
| **Quản lý giá trị** | Giá trị lặp lại cứng (`red`, `32px`, `#222`) | Tập trung trong `$variables` |
| **Tái sử dụng nút** | Viết lại từng thuộc tính `padding`, `border-radius` | Dùng `@mixin button-style($bg-color)` |
| **Tái sử dụng thẻ hộp** | Lặp lại thuộc tính bóng đổ, đệm lề | Dùng `@mixin box-style` |
| **Bảo trì / Thay đổi theme** | Sửa thủ công hàng chục chỗ | Thay đổi giá trị biến một lần duy nhất |

---

## 🛠️ Hướng dẫn biên dịch SASS

Cài đặt SASS (nếu chưa cài):
```bash
npm install -g sass
```

Biên dịch từ file `main.scss` sang `main.css`:
```bash
npm run compile:sass
# hoặc chạy trực tiếp:
sass main.scss main.css
```

Lắng nghe thay đổi tự động (watch mode):
```bash
npm run watch:sass
# hoặc chạy trực tiếp:
sass --watch main.scss main.css
```

---

## 🌐 Xem trực tiếp
Mở file `index.html` trực tiếp bằng trình duyệt hoặc dùng Live Server trong VS Code để xem giao diện hoàn chỉnh.
