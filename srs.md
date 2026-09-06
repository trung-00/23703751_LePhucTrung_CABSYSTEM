4. Phạm vi Module Hệ thống (System Modules)

Để đảm bảo dự án hoàn thành trong 7 tuần, CAB System được giới hạn trong 5 module chính. Các module tập trung vào quy trình đặt xe cốt lõi và các chức năng cần thiết cho vận hành.

4.1. Quản lý Khách hàng (Customer Management)

Mục đích:
Quản lý tài khoản và thông tin cơ bản của khách hàng.

Business Goal: BG-02, BG-04

Chức năng:

Đăng ký, đăng nhập, đăng xuất.
Xem và cập nhật thông tin cá nhân.
Quản lý trạng thái tài khoản.
Xem lịch sử chuyến đi.
4.2. Quản lý Tài xế (Driver Management)

Mục đích:
Quản lý thông tin, trạng thái hoạt động và khả năng nhận chuyến của tài xế.

Business Goal: BG-01, BG-04, BG-05

Chức năng:

Đăng nhập tài khoản tài xế.
Xem và cập nhật thông tin.
Chuyển trạng thái Online / Offline.
Chuyển trạng thái Available / Busy.
Cập nhật vị trí GPS.
Nhận, chấp nhận hoặc từ chối chuyến.
Xem lịch sử chuyến.
4.3. Đặt xe & Ghép tài xế (Booking & Matching)

Mục đích:
Là module cốt lõi của hệ thống, chịu trách nhiệm tạo yêu cầu đặt xe và tự động tìm tài xế phù hợp.

Business Goal: BG-01, BG-02, BG-05

Chức năng:

Nhập điểm đón và điểm đến.
Tạo yêu cầu đặt xe.
Tính giá dự kiến.
Tìm tài xế phù hợp.
Gửi đề xuất chuyến.
Xử lý Accept / Reject / Timeout.
Retry khi tài xế từ chối hoặc Timeout.
Xác nhận tài xế.
Hủy chuyến.
Quản lý trạng thái chuyến.

Luồng chính:

Đặt xe
   ↓
Tìm tài xế
   ↓
Gửi đề xuất
   ↓
Tài xế xác nhận
   ↓
Thực hiện chuyến
   ↓
Hoàn thành

4.4. Theo dõi Chuyến & Thanh toán (Ride Tracking & Payment)

Mục đích:
Theo dõi quá trình thực hiện chuyến và xử lý thanh toán khi chuyến hoàn thành.

Business Goal: BG-02, BG-03, BG-05

Chức năng:

Cập nhật vị trí GPS tài xế.
Hiển thị vị trí tài xế cho khách hàng.
Cập nhật trạng thái chuyến.
Xác nhận đến điểm đón.
Xác nhận đón khách.
Bắt đầu chuyến.
Hoàn thành chuyến.
Tính cước.
Tạo và xử lý giao dịch thanh toán.
Lưu trạng thái thanh toán.
Xử lý thanh toán thất bại.
4.5. Quản trị & Vận hành (Admin & Operation)

Mục đích:
Hỗ trợ nhân viên vận hành quản lý dữ liệu và giám sát hoạt động của hệ thống.

Business Goal: BG-03, BG-04, BG-05

Chức năng:

Đăng nhập Admin / Operator.
Quản lý khách hàng.
Quản lý tài xế.
Theo dõi trạng thái tài xế.
Giám sát chuyến đi.
Xem chi tiết chuyến.
Hỗ trợ xử lý chuyến lỗi.
Xem giao dịch thanh toán.
Xem báo cáo cơ bản.
5. Liên kết Module với Business Goal
Module	Business Goal
Quản lý Khách hàng	BG-02, BG-04
Quản lý Tài xế	BG-01, BG-04, BG-05
Đặt xe & Ghép tài xế	BG-01, BG-02, BG-05
Theo dõi Chuyến & Thanh toán	BG-02, BG-03, BG-05
Quản trị & Vận hành	BG-03, BG-04, BG-05
Ghi chú
BG-01: Tự động hóa ghép chuyến → tập trung ở Booking & Matching.
BG-02: Nâng cao trải nghiệm khách hàng → Customer, Booking và Tracking.
BG-03: Quản lý thanh toán → Payment và Admin/Operation.
BG-04: Tối ưu vận hành → Customer, Driver và Admin/Operation.
BG-05: Khả năng chịu tải và ổn định → áp dụng cho toàn hệ thống.
BG-06: Hoàn thành MVP trong 7 tuần → là mục tiêu của toàn dự án.
BG-07: Khả năng mở rộng kiến trúc → là mục tiêu của kiến trúc toàn hệ thống.
6. Giới hạn MVP

Trong thời gian 7 tuần, hệ thống chỉ tập trung vào luồng nghiệp vụ chính:

Customer
   │
   ▼
Đặt xe
   │
   ▼
Booking & Matching
   │
   ▼
Driver
   │
   ▼
Ride Tracking
   │
   ▼
Payment
   │
   ▼
Hoàn tất chuyến


Admin / Operator có nhiệm vụ quản lý và giám sát các thành phần trên.

Các chức năng nâng cao như Loyalty, Voucher, Carpooling, Dynamic Pricing, AI/ML, BI nâng cao và quản lý tài chính chuyên sâu không thuộc phạm vi MVP.
