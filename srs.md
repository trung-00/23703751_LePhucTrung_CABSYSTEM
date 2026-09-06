# SOFTWARE REQUIREMENTS SPECIFICATION (SRS)
## Dự án: Hệ thống Đặt xe Trực tuyến (CAB System)

* **Tác giả:** Lê Phúc Trung
* **Mã số sinh viên:** 23703751
* **Dự án:** CAB System - Nền tảng đặt xe
* **Thời gian thực hiện dự án:** 7 tuần

---

## 1. Giới thiệu (Introduction)

### 1.1. Mục đích
Tài liệu Đặc tả Yêu cầu Phần mềm (SRS) này mô tả chi tiết các yêu cầu chức năng, yêu cầu phi chức năng, quy tắc nghiệp vụ và kiến trúc tổng quan cho dự án **CAB System** của Công ty ABC. Tài liệu đóng vai trò làm cơ sở để nhóm phát triển, kiểm thử (QA/QC) và các bên liên quan (Stakeholders) triển khai sản phẩm đúng tiến độ 7 tuần.

### 1.2. Bối cảnh & Báo cáo bài toán
Công ty ABC hiện kinh doanh dịch vụ đặt xe trực tuyến nhưng gặp nhiều hạn chế:
* Phân công tài xế chủ yếu thực hiện thủ công.
* Khách hàng khó theo dõi trạng thái chuyến đi theo thời gian thực.
* Thông tin thanh toán chưa được quản lý tập trung.
* Khó mở rộng hệ thống khi số lượng chuyến đi tăng cao.

### 1.3. Mục tiêu dự án
* Xây dựng nền tảng CAB hiện đại, linh hoạt, phục vụ quy mô lớn khách hàng và tài xế.
* Tự động hóa quy trình ghép xe (Matching Engine), thanh toán và gửi thông báo.
* Đảm bảo kiến trúc có khả năng mở rộng độc lập, cho phép bổ sung dịch vụ/phương thức thanh toán mới trong tương lai.

---

## 2. Bên liên quan & Tác nhân Hệ thống (Stakeholders & Actors)

### 2.1. Danh sách Stakeholders
* **Ban Giám Đốc:** Sponsor dự án, theo dõi tiến độ 7 tuần, xem báo cáo hiệu quả kinh doanh.
* **Bộ phận Vận hành:** Quản lý tài xế, khách hàng, giám sát chuyến đi và xử lý sự cố.
* **Bộ phận Kế toán:** Quản lý doanh thu, đối soát thanh toán.
* **Đội ngũ Phát triển (BA, Dev, QA):** Phân tích và xây dựng hệ thống.
* **Nhà cung cấp Thanh toán (PSP):** Tích hợp cổng thanh toán điện tử bên thứ ba.
* **Nhà cung cấp Thông báo:** Đối tác hạ tầng SMS / Push Notification.

### 2.2. Các Tác nhân Hệ thống (Actors)
1. **Khách hàng (Customer):** Đặt xe, theo dõi chuyến đi, thanh toán, đánh giá tài xế.
2. **Tài xế (Driver):** Định vị GPS, nhận/từ chối cuốc xe, cập nhật trạng thái chuyến đi.
3. **Nhân viên vận hành (Admin/Operator):** Quản lý hệ thống, phân quyền, hỗ trợ chuyến đi lỗi, xem báo cáo.

---

## 3. Quy trình Nghiệp vụ Cốt lõi (Core Business Process)
