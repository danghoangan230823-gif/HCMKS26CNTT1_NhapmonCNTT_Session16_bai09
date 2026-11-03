# Bài 9: Thiết Kế Không Gian Đa Profile Và Kiến Trúc Bookmark Cho Trạm Khám Bệnh
## **Phần 1: Báo cáo phân tích kiến trúc đa Profile**
### 1.Nguyên nhân văn phiên:
Chrome lưu cookie đăng nhập trong thư mục **User Data** của từng Profile (file `Network\Cookies`). Khi cả phòng khám dùng chung 1 Profile, mọi tab đều **dùng chung một kho cookie**.

Khi thực tập sinh đăng nhập tài khoản khác (Google, GitHub, PTIT), cookie mới có thể **ghi đè** hoặc làm mất cookie đăng nhập CarePulse. Server không nhận ra phiên cũ nên đưa về trang đăng nhập, bác sĩ bị văng giữa lúc kê đơn.

**Kết luận:** nguyên nhân là không tách riêng dữ liệu. Mỗi vai trò dùng 1 Profile riêng thì cookie không lẫn vào nhau, hết văng phiên.
### 2.Mô hình 3 Proflie:
| | Work | Personal | Sandbox/Test |
|---|---|---|---|
| Tên |  CarePulse-Work |  Personal |  Sandbox-Test |
| Mục đích | CarePulse, BHYT, Dược thư | Học tập, GitHub, MXH, mua sắm | Môi trường test, thử extension, link lạ |
| Theme | Xanh dương | Xanh lá | Đỏ cam |
| Cookies | Chỉ phiên nghiệp vụ, xóa khi đóng | Phiên cá nhân | Tài khoản test, xóa khi đóng |
| History | Nghiệp vụ, tắt gợi ý cá nhân | Riêng, không lên máy chiếu | Xóa định kỳ |
| Bookmarks | Cây 3 thư mục | Riêng từng người | Link staging, API |
| Extensions | Tối thiểu, đã duyệt | Tự do | Thử thoải mái |

Profile Work: tắt Password Manager, Autofill và Sync. Mỗi người một tài khoản Windows nếu có thể.
## Phần 2: Bookmark & an toàn

### 3. Cây Bookmark (`Ctrl+Shift+O`), 30 link
- **01_CarePulse_Clinic**: Dashboard, Hồ sơ bệnh nhân, Kê đơn, Lịch khám, Xét nghiệm, Báo cáo ca trực, Kho thuốc, Chuyển tuyến, Hướng dẫn nội bộ, IT Helpdesk
- **02_Pharma_Portal**: Dược thư Quốc gia, Cục Quản lý Dược, Tra cứu số đăng ký thuốc, Giám định BHYT, BHXH VN, Danh mục thuốc BHYT, Drugs.com, Medscape Interaction Checker, Thuốc thu hồi, Dịch vụ công QG
- **03_Medical_Tools**: Google Dịch, Bộ Y tế, ICD-10/11 (WHO), MDCalc, MSD Manual, PubMed, WHO Guidelines, UpToDate/BMJ, Tính liều trẻ em, Cẩm nang sơ cấp cứu


### 4. Favicon-Only
1. Mở trang, `Ctrl+D` lưu lên thanh dấu trang (`Ctrl+Shift+B` để hiện thanh).
2. Chuột phải bookmark → **Edit** → **xóa trống ô Name** → Save.
3. Làm cho CarePulse, Cổng BHYT, Dược thư, Google Dịch.

Mỗi link từ khoảng 120–150 px xuống khoảng 30–36 px, tiết kiệm khoảng 3–4 lần. Nếu hai icon giống nhau, đặt tên là 1 emoji.

### 5. Bẫy an ninh
**Dùng Guest Profile**, không dùng Incognito. Guest là phiên tách hẳn, không thấy bookmark, mật khẩu, autofill và đăng nhập của nhân viên, đóng là xóa sạch. Incognito vẫn chạy từ Profile hiện tại nên khách dễ chuyển sang cửa sổ thường đang đăng nhập CarePulse.

Quy trình: đăng xuất mọi hệ thống → đóng Profile Work/Personal → mở Guest → có người giám sát → xong đóng Guest và ghi sổ ca.

**Nguy cơ bật Cloud Sync cá nhân trên máy y tế:**
- Mật khẩu, lịch sử của người trước lộ cho người sau hoặc khách.
- URL, autofill chứa dữ liệu bệnh nhân bị đẩy lên Google Account cá nhân, vi phạm bảo mật y tế và Nghị định 13/2023/NĐ-CP.
- Quên đăng xuất thì người sau thừa hưởng toàn bộ phiên và dữ liệu.
- Extension cá nhân đồng bộ vào Profile Work có thể đọc hồ sơ.
- Mất mạng: dữ liệu chờ đồng bộ nằm trên máy rồi đẩy lên cloud khi có mạng.

**Khuyến nghị:** tắt Sync ở Profile Work. Nếu cần, IT dùng tài khoản đơn vị và chỉ đồng bộ Bookmarks.
Khi mất Internet: không lưu mật khẩu hay dữ liệu bệnh nhân trong trình duyệt, bật "xóa cookie khi đóng cửa sổ", hết ca đăng xuất và khóa Windows.
