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


mermaid
stateDiagram-v2
    [*] --> KhoiTao: Khách hàng đặt xe
    KhoiTao --> TimTaiXe: Hệ thống tìm tài xế
    
    TimTaiXe --> ChoTaiXeXacNhan: Gửi đề xuất đến tài xế
    ChoTaiXeXacNhan --> TimTaiXe: Tài xế từ chối / Timeout (Retry)
    ChoTaiXeXacNhan --> HuyChuyen: Không tìm thấy tài xế
    
    ChoTaiXeXacNhan --> DaNhanChuyen: Tài xế nhận chuyến
    DaNhanChuyen --> DaDenDiemDon: Tài xế đến điểm đón
    DaDenDiemDon --> DaDonKhach: Khách lên xe
    DaDonKhach --> DangDiChuyen: Bắt đầu di chuyển
    DangDiChuyen --> HoanThanh: Đến điểm đến
    
    HoanThanh --> ThanhToan: Tính cước & Thanh toán
    ThanhToan --> [*]: Hoàn tất & Đánh giá

    DaNhanChuyen --> HuyChuyen: Khách / Tài xế hủy
    HuyChuyen --> [*]


## 8. Mục tiêu Nghiệp vụ (Business Goals - BG)

Chuyển đổi từ các vấn đề hiện tại và kỳ vọng của Ban lãnh đạo Công ty ABC thành các Mục tiêu Nghiệp vụ định lượng và định tính:

| Mã BG | Nhu cầu / Yêu cầu gốc của Khách hàng | Mục tiêu Nghiệp vụ (Business Goal) | Chỉ số đo lường (KPI / Target) |
| :--- | :--- | :--- | :--- |
| **BG-01** | *Việc phân công tài xế chủ yếu được thực hiện thủ công, mất thời gian.* | **Tự động hóa quy trình ghép chuyến (Matching Engine)** nhằm tối ưu tốc độ điều xe và giảm chi phí vận hành thủ công. | • Tự động hóa **100%** việc ghép chuyến.<br>• Thời gian tìm và đề xuất tài xế $< 10$ giây. |
| **BG-02** | *Khách hàng khó theo dõi trạng thái chuyến đi và vị trí tài xế.* | **Nâng cao trải nghiệm khách hàng (Customer Experience)** bằng cách minh bạch hóa lộ trình và trạng thái chuyến đi theo thời gian thực. | • **100%** chuyến đi được cập nhật vị trí GPS realtime.<br>• Tăng chỉ số hài lòng khách hàng (CSAT) lên **> 85%**. |
| **BG-03** | *Thông tin thanh toán chưa được quản lý tập trung và thiếu linh hoạt.* | **Tập trung hóa quản lý doanh thu & Tích hợp thanh toán đa kênh**, đảm bảo an toàn bảo mật dữ liệu tài chính. | • **100%** giao dịch được lưu vết tập trung.<br>• Tuân thủ tiêu chuẩn bảo mật PCI-DSS (không lưu dữ liệu thẻ nhạy cảm). |
| **BG-04** | *Bộ phận vận hành gặp khó khăn khi muốn mở rộng hệ thống và quản lý.* | **Tối ưu hóa năng lực quản trị vận hành**, hỗ trợ nhân viên giám sát thời gian thực, xử lý sự cố nhanh và báo cáo tự động. | • Giảm **50%** thời gian xử lý khiếu nại/chuyến lỗi.<br>• Báo cáo doanh thu/vận hành tự động xuất real-time. |
| **BG-05** | *Hệ thống cần phục vụ số lượng lớn khách hàng/tài xế và hoạt động ổn định khi cao điểm.* | **Xây dựng hạ tầng có độ sẵn sàng cao (High Availability)** và khả năng chịu tải tốt, không bị gián đoạn toàn bộ khi xảy ra lỗi thành phần. | • Uptime hệ thống đạt **99.9%**.<br>• Lỗi module Thanh toán/Thông báo không làm gián đoạn luồng Đặt xe (Fault Isolation). |
| **BG-06** | *Thời gian xây dựng và triển khai sản phẩm trong vòng 7 tuần.* | **Tối ưu hóa thời gian đưa sản phẩm ra thị trường (Time-to-Market)** với chiến lược phát triển MVP đúng hạn. | • Triển khai thành công phiên bản MVP hoạt động ổn định trong đúng **7 tuần**. |
| **BG-07** | *Kiến trúc đủ linh hoạt để tương lai bổ sung dịch vụ, thanh toán, thông báo mới.* | **Đảm bảo tính linh hoạt & Khả năng mở rộng kiến trúc (Scalability & Extensibility)** cho định hướng phát triển dài hạn. | • Chi phí và thời gian tích hợp thêm 1 kênh thanh toán/thông báo mới $< 1$ tuần phát triển. |
