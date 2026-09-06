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


23. Mục tiêu Nghiệp vụ (Business Goals - BG)

Chuyển đổi từ các vấn đề hiện tại và kỳ vọng của Ban lãnh đạo Công ty ABC thành các Mục tiêu Nghiệp vụ định lượng và định tính:

Mã BG	Nhu cầu / Yêu cầu gốc của Khách hàng	Mục tiêu Nghiệp vụ (Business Goal)	Chỉ số đo lường (KPI / Target)
BG-01	Việc phân công tài xế chủ yếu được thực hiện thủ công, mất thời gian.	Tự động hóa quy trình ghép chuyến (Matching Engine) nhằm tối ưu tốc độ điều xe và giảm chi phí vận hành thủ công.	• Tự động hóa 100% việc ghép chuyến.<br>• Thời gian tìm và đề xuất tài xế < 10 giây.
BG-02	Khách hàng khó theo dõi trạng thái chuyến đi và vị trí tài xế.	Nâng cao trải nghiệm khách hàng (Customer Experience) bằng cách minh bạch hóa lộ trình và trạng thái chuyến đi theo thời gian thực.	• 100% chuyến đi được cập nhật vị trí GPS realtime.<br>• Tăng chỉ số hài lòng khách hàng (CSAT) lên > 85%.
BG-03	Thông tin thanh toán chưa được quản lý tập trung và thiếu linh hoạt.	Tập trung hóa quản lý doanh thu & Tích hợp thanh toán đa kênh, đảm bảo an toàn bảo mật dữ liệu tài chính.	• 100% giao dịch được lưu vết tập trung.<br>• Tuân thủ tiêu chuẩn bảo mật PCI-DSS (không lưu dữ liệu thẻ nhạy cảm).
BG-04	Bộ phận vận hành gặp khó khăn khi muốn mở rộng hệ thống và quản lý.	Tối ưu hóa năng lực quản trị vận hành, hỗ trợ nhân viên giám sát thời gian thực, xử lý sự cố nhanh và báo cáo tự động.	• Giảm 50% thời gian xử lý khiếu nại/chuyến lỗi.<br>• Báo cáo doanh thu/vận hành tự động xuất realtime.
BG-05	Hệ thống cần phục vụ số lượng lớn khách hàng/tài xế và hoạt động ổn định khi cao điểm.	Xây dựng hạ tầng có độ sẵn sàng cao (High Availability) và khả năng chịu tải tốt, không bị gián đoạn toàn bộ khi xảy ra lỗi thành phần.	• Uptime hệ thống đạt 99.9%.<br>• Lỗi module Thanh toán/Thông báo không làm gián đoạn luồng Đặt xe (Fault Isolation).
BG-06	Thời gian xây dựng và triển khai sản phẩm trong vòng 7 tuần.	Tối ưu hóa thời gian đưa sản phẩm ra thị trường (Time-to-Market) với chiến lược phát triển MVP đúng hạn.	• Triển khai thành công phiên bản MVP hoạt động ổn định trong đúng 7 tuần.
BG-07	Kiến trúc đủ linh hoạt để tương lai bổ sung dịch vụ, thanh toán, thông báo mới.	Đảm bảo tính linh hoạt & Khả năng mở rộng kiến trúc (Scalability & Extensibility) cho định hướng phát triển dài hạn.	• Chi phí và thời gian tích hợp thêm 1 kênh thanh toán/thông báo mới < 1 tuần phát triển.
| **BG-06** | *Thời gian xây dựng và triển khai sản phẩm trong vòng 7 tuần.* | **Tối ưu hóa thời gian đưa sản phẩm ra thị trường (Time-to-Market)** với chiến lược phát triển MVP đúng hạn. | • Triển khai thành công phiên bản MVP hoạt động ổn định trong đúng **7 tuần**. |


4. Phạm vi Module Hệ thống (System Modules)

Để đảm bảo dự án CAB System có thể hoàn thành trong thời gian 7 tuần, hệ thống được giới hạn trong 5 module chính. Mỗi module được xây dựng để đáp ứng trực tiếp một hoặc nhiều Business Goal đã xác định.

4.1. Module 1 - Quản lý Khách hàng (Customer Management)
Mục đích

Quản lý tài khoản và thông tin cơ bản của khách hàng, phục vụ quá trình đặt xe và sử dụng dịch vụ.

Business Goal liên quan
BG-02: Nâng cao trải nghiệm khách hàng.
BG-04: Tối ưu hóa năng lực quản trị vận hành.
Chức năng trong phạm vi MVP
Đăng ký tài khoản.
Đăng nhập/đăng xuất.
Xem thông tin cá nhân.
Cập nhật thông tin cá nhân.
Quản lý trạng thái tài khoản.
Xem lịch sử chuyến đi.
Không nằm trong MVP
Chương trình khách hàng thân thiết.
Tích điểm.
Voucher nâng cao.
Marketing Automation.
Phân tích hành vi khách hàng bằng AI.
4.2. Module 2 - Quản lý Tài xế (Driver Management)
Mục đích

Quản lý thông tin tài xế và trạng thái hoạt động, làm cơ sở cho việc tự động ghép tài xế với khách hàng.

Business Goal liên quan
BG-01: Tự động hóa quy trình ghép chuyến.
BG-04: Tối ưu hóa năng lực quản trị vận hành.
BG-05: Đảm bảo hệ thống hoạt động ổn định và có khả năng mở rộng.
Chức năng trong phạm vi MVP
Đăng nhập tài khoản tài xế.
Xem/cập nhật thông tin tài xế.
Cập nhật trạng thái Online/Offline.
Cập nhật trạng thái Available/Busy.
Cập nhật vị trí GPS.
Nhận đề xuất chuyến.
Chấp nhận chuyến.
Từ chối chuyến.
Xem lịch sử chuyến.
Không nằm trong MVP
Quản lý hồ sơ pháp lý nâng cao.
Quản lý bảo hiểm.
Tính lương tài xế.
Quản lý thưởng/phạt nâng cao.
Quản lý đội xe chuyên sâu.
4.3. Module 3 - Đặt xe & Ghép tài xế (Booking & Matching)
Mục đích

Đây là module nghiệp vụ cốt lõi của CAB System, chịu trách nhiệm tiếp nhận yêu cầu đặt xe và tự động tìm tài xế phù hợp.

Business Goal liên quan
BG-01: Tự động hóa quy trình ghép chuyến.
BG-02: Nâng cao trải nghiệm khách hàng.
BG-05: Đảm bảo khả năng chịu tải và hoạt động ổn định.
Chức năng trong phạm vi MVP
Nhập điểm đón.
Nhập điểm đến.
Tạo yêu cầu đặt xe.
Tính giá dự kiến.
Tìm tài xế phù hợp.
Gửi đề xuất chuyến cho tài xế.
Xử lý tài xế chấp nhận/từ chối.
Xử lý Timeout.
Retry tìm tài xế.
Xác nhận tài xế.
Hủy chuyến.
Quản lý trạng thái chuyến.
Trạng thái chuyến
KHOI_TAO
    ↓
TIM_TAI_XE
    ↓
CHO_TAI_XE_XAC_NHAN
    ↓
DA_NHAN_CHUYEN
    ↓
DA_DEN_DIEM_DON
    ↓
DA_DON_KHACH
    ↓
DANG_DI_CHUYEN
    ↓
HOAN_THANH


Nhánh hủy:

CHO_TAI_XE_XAC_NHAN
        ↓
    HUY_CHUYEN


hoặc:

DA_NHAN_CHUYEN
        ↓
    HUY_CHUYEN

Không nằm trong MVP
Thuật toán giá động phức tạp.
Carpooling.
Đặt nhiều chuyến cùng lúc.
Đặt xe theo lịch dài hạn.
Tối ưu tuyến đường bằng AI.
4.4. Module 4 - Theo dõi Chuyến đi & Thanh toán (Ride Tracking & Payment)
Mục đích

Quản lý quá trình thực hiện chuyến đi, cập nhật vị trí tài xế và xử lý thanh toán sau khi chuyến hoàn thành.

Business Goal liên quan
BG-02: Nâng cao trải nghiệm khách hàng.
BG-03: Tập trung hóa quản lý doanh thu và thanh toán.
BG-05: Đảm bảo Fault Isolation.
Chức năng trong phạm vi MVP
Theo dõi chuyến
Cập nhật vị trí GPS tài xế.
Hiển thị vị trí tài xế cho khách hàng.
Cập nhật trạng thái chuyến.
Tài xế xác nhận đã đến điểm đón.
Tài xế xác nhận đã đón khách.
Tài xế bắt đầu chuyến.
Tài xế hoàn thành chuyến.
Thanh toán
Tính cước chuyến.
Chọn phương thức thanh toán.
Tạo giao dịch.
Gửi giao dịch đến Payment Provider.
Nhận kết quả thanh toán.
Lưu trạng thái giao dịch.
Xử lý thanh toán thất bại.
Không nằm trong MVP
Ví điện tử nội bộ.
Hệ thống trả góp.
Loyalty Payment.
Đối soát tài chính nâng cao.
Dynamic Pricing phức tạp.
4.5. Module 5 - Quản trị & Vận hành (Admin & Operation)
Mục đích

Cung cấp công cụ cho nhân viên vận hành quản lý khách hàng, tài xế và giám sát chuyến đi.

Business Goal liên quan
BG-04: Tối ưu hóa năng lực quản trị vận hành.
BG-03: Tập trung hóa
| **BG-07** | *Kiến trúc đủ linh hoạt để tương lai bổ sung dịch vụ, thanh toán, thông báo mới.* | **Đảm bảo tính linh hoạt & Khả năng mở rộng kiến trúc (Scalability & Extensibility)** cho định hướng phát triển dài hạn. | • Chi phí và thời gian tích hợp thêm 1 kênh thanh toán/thông báo mới $< 1$ tuần phát triển. |
