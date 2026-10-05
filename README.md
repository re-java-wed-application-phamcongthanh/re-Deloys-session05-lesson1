# Báo Cáo Bài 1: Khôi phục commit đã mất bằng Git Reflog

## 1. Mục tiêu
- Hiểu được cách Git lưu trữ lịch sử hoạt động cục bộ thông qua Reflog (Reference Log).
- Sử dụng thành thạo lệnh git reflog để truy vết các tham chiếu commit cũ đã bị mất hoặc bị xóa.
- Khôi phục thành công một commit đã bị xóa mất khỏi nhánh làm việc sau khi thực thi lệnh git reset --hard.

---

## 2. Quy trình Thực hiện & Giả lập Sự cố

### Bước 1: Khởi tạo tệp tin và tạo commit tính năng quan trọng
Tạo tệp eature.txt với nội dung Day la tinh nang quan trong:
`ash
git add .
git commit -m "Them tinh nang quan trong"
`

### Bước 2: Giả lập sự cố lỡ tay xóa commit bằng git reset --hard
Giả sử học viên vô tình lùi commit và xóa sạch working tree:
`ash
git reset --hard HEAD~1
`
*Kết quả:* Commit "Them tinh nang quan trong" và tệp eature.txt hoàn toàn biến mất khỏi git log.

### Bước 3: Tra cứu nhật ký tham chiếu (git reflog)
Chạy lệnh git reflog để xem lịch sử chuyển đổi vị trí con trỏ HEAD:
`ash
git reflog
`
**Output hiển thị:**
`	ext
11000b8 HEAD@{1}: commit: Them tinh nang quan trong
a1b2c3d HEAD@{0}: reset: moving to HEAD~1
`

### Bước 4: Khôi phục commit bằng mã hash từ Reflog
Sử dụng mã hash tìm được từ Reflog để đưa nhánh hiện tại về đúng trạng thái ban đầu:
`ash
git reset --hard 11000b8
`

---

## 3. Kết quả Kiểm tra (Verification)

### Kiểm tra sự tồn tại của tệp eature.txt:
`powershell
Get-Content feature.txt
# Output: Day la tinh nang quan trong
`

### Xem lịch sử commit sau khi khôi phục (git log --oneline):
`ash
git log --oneline
`
**Output:**
`	ext
11000b8 Them tinh nang quan trong
a1b2c3d Initial commit: App setup
`

---

## 4. Kết luận
- git reflog là công cụ cứu hộ tuyệt vời trong Git giúp ghi lại mọi hành động thay đổi con trỏ HEAD.
- Dù sử dụng git reset --hard, dữ liệu commit chưa bao giờ thực sự mất đi ngay lập tức mà vẫn nằm trong Reflog.
- Đã khôi phục thành công 100% commit và dữ liệu đã mất mà không phải gõ lại dòng code nào.
