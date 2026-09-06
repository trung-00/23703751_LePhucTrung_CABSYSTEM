4. Phạm vi Module Hệ thống (System Modules)

Để đảm bảo hoàn thành MVP trong 7 tuần, CAB System được giới hạn trong 5 module chính. Mỗi module được liên kết trực tiếp với các Business Goal tương ứng.

STT	Module	Business Goal chính
1	Quản lý Khách hàng	BG-02, BG-04
2	Quản lý Tài xế	BG-01, BG-04, BG-05
3	Đặt xe & Ghép tài xế	BG-01, BG-02, BG-05
4	Theo dõi Chuyến & Thanh toán	BG-02, BG-03, BG-05
5	Quản trị & Vận hành	BG-03, BG-04, BG-05

BG-06 và BG-07 là mục tiêu ở cấp độ toàn hệ thống, không tạo thành module riêng.

4.1. Module 1 - Quản lý Khách hàng (Customer Management)

Mục đích:
Quản lý tài khoản và thông tin cơ bản của khách hàng, phục vụ quá trình đặt và sử dụng dịch vụ.

Business Goal: BG-02, BG-04

Chức năng MVP:

Đăng ký / đăng nhập / đăng xuất.
Xem và cập nhật thông tin cá nhân.
Quản lý trạng thái tài khoản.
Xem lịch sử chuyến đi.

Ngoài phạm vi MVP:

Loyalty / tích điểm.
Voucher nâng cao.
Marketing Automation.
Phân tích hành vi bằng AI.
4.2. Module 2 - Quản lý Tài xế (Driver Management)

Mục đích:
Quản lý thông tin và trạng thái hoạt động của tài xế, làm cơ sở cho việc ghép chuyến.

Business Goal: BG-01, BG-04, BG-05

Chức năng MVP:

Đăng nhập tài khoản tài xế.
Xem / cập nhật thông tin.
Online / Offline.
Available / Busy.
Cập nhật vị trí GPS.
Nhận, chấp nhận hoặc từ chối chuyến.
Xem lịch sử chuyến.

Ngoài phạm vi MVP:

Quản lý hồ sơ pháp lý nâng cao.
Bảo hiểm.
Tính lương, thưởng/phạt.
Quản lý đội xe chuyên sâu.
4.3. Module 3 - Đặt xe & Ghép tài xế (Booking & Matching)

Mục đích:
Là module cốt lõi, chịu trách nhiệm tiếp nhận yêu cầu đặt xe và tự động tìm tài xế phù hợp.

Business Goal: BG-01, BG-02, BG-05

Chức năng MVP:

Nhập điểm đón và điểm đến.
Tạo yêu cầu đặt xe.
Tính giá dự kiến.
Tìm tài xế phù hợp.
Gửi đề xuất chuyến.
Xử lý Accept / Reject / Timeout.
Retry tìm tài xế.
Xác nhận tài xế.
Hủy chuyến.
Quản lý trạng thái chuyến.

Luồng trạng thái chính:

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


Luồng hủy:

CHO_TAI_XE_XAC_NHAN ──→ HUY_CHUYEN
DA_NHAN_CHUYEN ────────→ HUY_CHUYEN


Ngoài phạm vi MVP:

Dynamic Pricing phức tạp.
Carpooling.
Đặt nhiều chuyến đồng thời.
Đặt xe theo lịch.
Tối ưu tuyến đường bằng AI.
4.4. Module 4 - Theo dõi Chuyến & Thanh toán (Ride Tracking & Payment)

Mục đích:
Theo dõi quá trình thực hiện chuyến và xử lý thanh toán sau khi chuyến hoàn thành.

Business Goal: BG-02, BG-03, BG-05

Theo dõi chuyến
Cập nhật vị trí GPS.
Hiển thị vị trí tài xế.
Cập nhật trạng thái chuyến.
Xác nhận đến điểm đón.
Xác nhận đón khách.
Bắt đầu chuyến.
Hoàn thành chuyến.
Thanh toán
Tính cước.
Chọn phương thức thanh toán.
Tạo giao dịch.
Gửi giao dịch đến Payment Provider.
Nhận kết quả thanh toán.
Lưu trạng thái giao dịch.
Xử lý thanh toán thất bại.

Ngoài phạm vi MVP:

Ví điện tử nội bộ.
Trả góp.
Loyalty Payment.
Đối soát tài chính nâng cao.
Dynamic Pricing phức tạp.
4.5. Module 5 - Quản trị & Vận hành (Admin & Operation)

Mục đích:
Cung cấp công cụ cho Admin/Operator quản lý dữ liệu và giám sát hoạt động của hệ thống.

Business Goal: BG-03, BG-04, BG-05

Chức năng MVP:

Đăng nhập Admin/Operator.
Quản lý khách hàng.
Quản lý tài xế.
Xem trạng thái tài xế.
Giám sát chuyến đi.
Xem chi tiết chuyến.
Hỗ trợ xử lý chuyến lỗi.
Xem giao dịch thanh toán.
Xem báo cáo cơ bản.

Báo cáo MVP:

Tổng số chuyến.
Chuyến hoàn thành / hủy.
Tổng doanh thu.
Số khách hàng.
Số tài xế.
Tỷ lệ nhận chuyến.

Ngoài phạm vi MVP:

BI Dashboard nâng cao.
Phân tích dữ liệu bằng AI/ML.
Dự báo doanh thu.
Workforce Management nâng cao.
5. Ma trận Module - Business Goal
Module	BG-01	BG-02	BG-03	BG-04	BG-05
Quản lý Khách hàng		✓		✓	
Quản lý Tài xế	✓			✓	✓
Đặt xe & Ghép tài xế	✓	✓			✓
Theo dõi Chuyến & Thanh toán		✓	✓		✓
Quản trị & Vận hành			✓	✓	✓
6. Giới hạn MVP

Trong phạm vi 7 tuần, hệ thống chỉ tập trung vào luồng nghiệp vụ cốt lõi:

Customer
   │
   │ Đặt xe
   ▼
Booking
   │
   ▼
Matching Engine
   │
   │ Ghép tài xế
   ▼
Driver
   │
   │ Nhận & thực hiện chuyến
   ▼
Ride Tracking
   │
   │ Hoàn thành
   ▼
Payment
   │
   ▼
Hoàn tất


Admin/Operator giám sát và hỗ trợ toàn bộ quy trình:

Admin / Operator
       │
       ├── Quản lý khách hàng
       ├── Quản lý tài xế
       ├── Giám sát chuyến đi
       └── Xem thanh toán & báo cáo

Phân loại Business Goal
Business Goal	Phạm vi
BG-01	Booking & Matching
BG-02	Customer + Booking + Tracking
BG-03	Payment + Admin/Operation
BG-04	Customer + Driver + Admin/Operation
BG-05	Toàn hệ thống
BG-06	Toàn dự án - hoàn thành MVP trong 7 tuần
BG-07	Kiến trúc toàn hệ thống - khả năng mở rộng

Nguyên tắc giới hạn: Không tạo module riêng cho BG-05, BG-06 và BG-07. Đây là các mục tiêu xuyên suốt toàn hệ thống.
