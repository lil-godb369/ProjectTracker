# Project Tracker — Hệ thống Quản lý Đồ án

> Ứng dụng desktop quản lý đồ án nhóm có **phân quyền theo vai trò**, **nhật ký kiểm toán (audit log)** và
> **bảng theo dõi tiến độ**. Viết bằng Python + CustomTkinter + SQLite.

Đồ án nhóm môn học tại **Trường Đại học Công Thương TP. Hồ Chí Minh (HUIT)**.

---

## Tính năng

### Quản lý công việc
- Tạo, sửa, xóa đồ án và công việc; theo dõi trạng thái hoàn thành.
- **Ma trận Eisenhower** — phân loại công việc theo mức độ quan trọng / khẩn cấp.
- **Timeline** — xem tiến độ theo dòng thời gian.
- **Biểu đồ** — thống kê tỷ lệ hoàn thành theo tuần (Matplotlib).

### Phân quyền & bảo mật
- Đăng nhập bằng tài khoản, mật khẩu được **băm SHA-256** trước khi lưu.
- **3 vai trò** với phạm vi dữ liệu riêng biệt (nguyên tắc đặc quyền tối thiểu):

| Vai trò | Quyền hạn |
|---|---|
| **Admin** (quản trị viên) | Toàn bộ hệ thống: quản lý người dùng, xem audit log, xuất/sao lưu dữ liệu |
| **Leader** (trưởng nhóm) | Quản lý đồ án và công việc trong nhóm mình phụ trách |
| **Member** (thành viên) | Chỉ xem và cập nhật công việc được giao |

- **Audit log** — ghi lại mọi thao tác quan trọng (đăng nhập, tạo/sửa/xóa, xuất báo cáo) kèm người thực hiện
  và thời gian; chỉ Admin xem được.

### Công cụ hệ thống (Admin)
- Quản lý tài khoản: tạo, khóa/mở khóa, đổi vai trò.
- Xuất báo cáo ra **CSV / Excel**.
- **Sao lưu và phục hồi** cơ sở dữ liệu.

---

## Kiến trúc

Áp dụng mô hình **MVVM** (Model – View – ViewModel) để tách logic nghiệp vụ khỏi giao diện:

```
main.py                 # Điểm khởi chạy, điều phối màn hình
├── models/             # Truy cập dữ liệu (user, project, task, audit log)
├── viewmodels/         # Logic nghiệp vụ, kiểm tra quyền, xử lý dữ liệu
├── views/              # Giao diện CustomTkinter + lớp database
│   └── database.py     # Khởi tạo schema, hash mật khẩu, kết nối SQLite
└── database.db         # CSDL SQLite (tự sinh khi chạy lần đầu)
```

---

## Cài đặt & chạy

```bash
# 1. Tạo môi trường ảo (khuyến nghị)
python -m venv .venv
.venv\Scripts\activate        # Windows
# source .venv/bin/activate   # macOS / Linux

# 2. Cài thư viện
pip install -r requirements.txt

# 3. Chạy ứng dụng
python main.py
```

Ứng dụng tự tạo file `database.db` và tài khoản Admin mặc định ở lần chạy đầu tiên.

> ⚠️ **Đổi mật khẩu Admin mặc định** ngay sau khi đăng nhập lần đầu.

---

## Công nghệ

| Thành phần | Thư viện |
|---|---|
| Giao diện | CustomTkinter (Tkinter) |
| Cơ sở dữ liệu | SQLite (`sqlite3`) |
| Xử lý dữ liệu | pandas |
| Biểu đồ | Matplotlib |
| Xuất báo cáo | openpyxl (Excel), csv |

---

## Ghi chú bảo mật

Đây là đồ án học tập, một số điểm **chưa đạt chuẩn production** và được ghi lại để minh bạch:

- Mật khẩu dùng SHA-256 **không kèm salt** — nên nâng lên `bcrypt` / `argon2` nếu triển khai thật.
- File `database.db` **không được commit** (đã có trong `.gitignore`) để tránh lộ dữ liệu người dùng.
- Mật khẩu Admin mặc định chỉ dùng cho môi trường demo.

---

## Tác giả

**Trần Ngọc Đăng Khoa** — Sinh viên An ninh hệ thống, HUIT
[github.com/lil-godb369](https://github.com/lil-godb369)
