SOFTWARE REQUIREMENTS SPECIFICATION (SRS)
Dự án: Hệ thống Đặt xe Trực tuyến (CAB System)
Tác giả: Lê Phúc Trung
Mã số sinh viên: 23703751
Dự án: CAB System - Nền tảng đặt xe
Thời gian thực hiện dự án: 7 tuần
1. Giới thiệu (Introduction)
1.1. Mục đích

Tài liệu Đặc tả Yêu cầu Phần mềm (SRS) này mô tả chi tiết các yêu cầu chức năng, yêu cầu phi chức năng, quy tắc nghiệp vụ và kiến trúc tổng quan cho dự án CAB System của Công ty ABC. Tài liệu đóng vai trò làm cơ sở để nhóm phát triển, kiểm thử (QA/QC) và các bên liên quan (Stakeholders) triển khai sản phẩm đúng tiến độ 7 tuần.

1.2. Bối cảnh & Báo cáo bài toán

Công ty ABC hiện kinh doanh dịch vụ đặt xe trực tuyến nhưng gặp nhiều hạn chế:

Phân công tài xế chủ yếu thực hiện thủ công.
Khách hàng khó theo dõi trạng thái chuyến đi theo thời gian thực.
Thông tin thanh toán chưa được quản lý tập trung.
Khó mở rộng hệ thống khi số lượng chuyến đi tăng cao.
1.3. Mục tiêu dự án
Xây dựng nền tảng CAB hiện đại, linh hoạt, phục vụ quy mô lớn khách hàng và tài xế.
Tự động hóa quy trình ghép xe (Matching Engine), thanh toán và gửi thông báo.
Đảm bảo kiến trúc có khả năng mở rộng độc lập, cho phép bổ sung dịch vụ/phương thức thanh toán mới trong tương lai.
2. Bên liên quan & Tác nhân Hệ thống (Stakeholders & Actors)
2.1. Danh sách Stakeholders
Ban Giám Đốc: Sponsor dự án, theo dõi tiến độ 7 tuần, xem báo cáo hiệu quả kinh doanh.
Bộ phận Vận hành: Quản lý tài xế, khách hàng, giám sát chuyến đi và xử lý sự cố.
Bộ phận Kế toán: Quản lý doanh thu, đối soát thanh toán.
Đội ngũ Phát triển (BA, Dev, QA): Phân tích và xây dựng hệ thống.
Nhà cung cấp Thanh toán (PSP): Tích hợp cổng thanh toán điện tử bên thứ ba.
Nhà cung cấp Thông báo: Đối tác hạ tầng SMS / Push Notification.
2.2. Các Tác nhân Hệ thống (Actors)
Khách hàng (Customer): Đặt xe, theo dõi chuyến đi, thanh toán, đánh giá tài xế.
Tài xế (Driver): Định vị GPS, nhận/từ chối cuốc xe, cập nhật trạng thái chuyến đi.
Nhân viên vận hành (Admin/Operator): Quản lý hệ thống, phân quyền, hỗ trợ chuyến đi lỗi, xem báo cáo.
3. Quy trình Nghiệp vụ Cốt lõi (Core Business Process)

Quy trình đặt xe và thực hiện chuyến đi của CAB System:

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

4. Phạm vi Module Hệ thống (System Modules)

Để đảm bảo dự án CAB System có thể hoàn thành trong thời gian 7 tuần, hệ thống được giới hạn trong 5 module chính.

4.1. Module 1 - Quản lý Khách hàng (Customer Management)

Mục đích:
Quản lý tài khoản và thông tin cơ bản của khách hàng, phục vụ quá trình đặt xe và sử dụng dịch vụ.

Business Goal: BG-02, BG-04

Chức năng MVP:

Đăng ký tài khoản.
Đăng nhập / đăng xuất.
Xem thông tin cá nhân.
Cập nhật thông tin cá nhân.
Quản lý trạng thái tài khoản.
Xem lịch sử chuyến đi.
4.2. Module 2 - Quản lý Tài xế (Driver Management)

Mục đích:
Quản lý thông tin tài xế và trạng thái hoạt động, làm cơ sở cho việc tự động ghép tài xế với khách hàng.

Business Goal: BG-01, BG-04, BG-05

Chức năng MVP:

Đăng nhập tài khoản tài xế.
Xem / cập nhật thông tin tài xế.
Cập nhật trạng thái Online / Offline.
Cập nhật trạng thái Available / Busy.
Cập nhật vị trí GPS.
Nhận đề xuất chuyến.
Chấp nhận chuyến.
Từ chối chuyến.
Xem lịch sử chuyến.
4.3. Module 3 - Đặt xe & Ghép tài xế (Booking & Matching)

Mục đích:
Là module nghiệp vụ cốt lõi của CAB System, chịu trách nhiệm tiếp nhận yêu cầu đặt xe và tự động tìm tài xế phù hợp.

Business Goal: BG-01, BG-02, BG-05

Chức năng MVP:

Nhập điểm đón.
Nhập điểm đến.
Tạo yêu cầu đặt xe.
Tính giá dự kiến.
Tìm tài xế phù hợp.
Gửi đề xuất chuyến cho tài xế.
Xử lý tài xế chấp nhận / từ chối.
Xử lý Timeout.
Retry tìm tài xế.
Xác nhận tài xế.
Hủy chuyến.
Quản lý trạng thái chuyến.

Trạng thái chuyến:

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

CHO_TAI_XE_XAC_NHAN ──→ HUY_CHUYEN
DA_NHAN_CHUYEN ────────→ HUY_CHUYEN

4.4. Module 4 - Theo dõi Chuyến & Thanh toán (Ride Tracking & Payment)

Mục đích:
Quản lý quá trình thực hiện chuyến đi, cập nhật vị trí tài xế và xử lý thanh toán sau khi chuyến hoàn thành.

Business Goal: BG-02, BG-03, BG-05

Chức năng MVP:

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
4.5. Module 5 - Quản trị & Vận hành (Admin & Operation)

Mục đích:
Cung cấp công cụ cho nhân viên vận hành quản lý khách hàng, tài xế và giám sát chuyến đi.

Business Goal: BG-03, BG-04, BG-05

Chức năng MVP:

Đăng nhập Admin / Operator.
Quản lý khách hàng.
Quản lý tài xế.
Xem trạng thái Online / Offline của tài xế.
Xem danh sách chuyến đi.
Xem chi tiết chuyến đi.
Theo dõi chuyến đang hoạt động.
Hỗ trợ xử lý chuyến lỗi.
Xem giao dịch thanh toán.
Xem báo cáo cơ bản.
4.6. Tổng quan phạm vi Module

CAB System trong phạm vi MVP bao gồm:

                    CAB SYSTEM
                        │
        ┌───────────────┼───────────────┐
        │               │               │
   Customer          Driver          Admin
   Management       Management       & Operation
        │               │               │
        └───────────────┼───────────────┘
                        │
                Booking & Matching
                        │
                        ▼
                Ride Tracking
                        │
                        ▼
                    Payment


Giới hạn MVP: Không triển khai các chức năng nâng cao như Loyalty, Voucher nâng cao, Carpooling, Dynamic Pricing phức tạp, AI/ML, BI nâng cao và quản lý tài chính chuyên sâu.


5. Business Requirements (BR)

Phần này mô tả các yêu cầu nghiệp vụ mà hệ thống CAB System phải đáp ứng để hỗ trợ quy trình đặt xe, quản lý tài xế, theo dõi chuyến đi, thanh toán và vận hành hệ thống.

5.1. Danh sách Business Requirements
Mã	Business Requirement	Module
BG-01	Hệ thống phải cho phép khách hàng tạo yêu cầu chuyến đi bằng cách nhập điểm đón và điểm đến.	Quản lý Khách hàng / Đặt xe
BG-02	Hệ thống phải cho phép khách hàng lựa chọn tài xế được hệ thống đề xuất và xác nhận chuyến đi.	Đặt xe & Ghép tài xế
BG-03	Hệ thống phải tự động tìm kiếm và đề xuất tài xế phù hợp dựa trên trạng thái hoạt động và vị trí hiện tại.	Quản lý Tài xế / Matching
BG-04	Hệ thống phải cho phép tài xế nhận hoặc từ chối yêu cầu chuyến đi trong một khoảng thời gian xác định.	Quản lý Tài xế
BG-05	Hệ thống phải cho phép khách hàng theo dõi trạng thái chuyến đi và vị trí tài xế trong quá trình thực hiện chuyến.	Theo dõi Chuyến
BG-06	Hệ thống phải cho phép tài xế cập nhật trạng thái chuyến từ khi nhận chuyến đến khi hoàn thành hoặc hủy chuyến.	Quản lý Tài xế / Theo dõi Chuyến
BG-07	Hệ thống phải tự động tính cước chuyến dựa trên thông tin chuyến đi và cung cấp số tiền cần thanh toán cho khách hàng.	Thanh toán
BG-08	Hệ thống phải cho phép khách hàng thực hiện thanh toán và lưu lại trạng thái của giao dịch.	Thanh toán
BG-09	Hệ thống phải cho phép khách hàng xem lịch sử các chuyến đi và thông tin thanh toán tương ứng.	Quản lý Khách hàng
BG-10	Hệ thống phải cho phép nhân viên vận hành quản lý khách hàng, tài xế và theo dõi các chuyến đi đang hoạt động.	Quản trị & Vận hành
BG-11	Hệ thống phải cho phép nhân viên vận hành xem thông tin giao dịch và báo cáo cơ bản về số chuyến, chuyến hoàn thành, chuyến hủy và doanh thu.	Quản trị & Vận hành
BG-12	Hệ thống phải đảm bảo các chức năng đặt xe vẫn hoạt động khi dịch vụ thanh toán hoặc thông báo gặp sự cố tạm thời.	Toàn hệ thống
5.2. Chi tiết Business Requirements
BG-01 - Tạo yêu cầu chuyến đi

Mô tả:
Hệ thống phải cho phép khách hàng tạo yêu cầu chuyến đi bằng cách nhập thông tin điểm đón và điểm đến.

Điều kiện:

Khách hàng đã đăng nhập.
Điểm đón hợp lệ.
Điểm đến hợp lệ.

Kết quả:

Hệ thống tạo một yêu cầu chuyến đi.
Yêu cầu được chuyển sang trạng thái TIM_TAI_XE.
BG-02 - Lựa chọn và xác nhận tài xế

Mô tả:
Hệ thống phải cho phép khách hàng lựa chọn tài xế được hệ thống đề xuất và xác nhận chuyến đi.

Kết quả:

Tài xế được gán vào chuyến.
Chuyến chuyển sang trạng thái DA_NHAN_CHUYEN.
BG-03 - Tự động ghép tài xế

Mô tả:
Hệ thống phải tự động tìm kiếm và đề xuất tài xế phù hợp cho yêu cầu chuyến đi.

Tiêu chí lựa chọn:

Tài xế đang Online.
Tài xế đang Available.
Tài xế có vị trí GPS hợp lệ.
Tài xế phù hợp với khu vực điểm đón.

Kết quả:

Hệ thống gửi đề xuất chuyến đến tài xế.
Nếu tài xế từ chối hoặc Timeout, hệ thống tiếp tục tìm tài xế khác.
BG-04 - Tài xế nhận hoặc từ chối chuyến

Mô tả:
Hệ thống phải cho phép tài xế nhận hoặc từ chối yêu cầu chuyến đi trong thời gian quy định.

Kết quả:

Accept: Chuyến được xác nhận.
Reject: Hệ thống tìm tài xế khác.
Timeout: Hệ thống xem như tài xế không nhận chuyến và thực hiện Retry.
BG-05 - Theo dõi chuyến đi

Mô tả:
Hệ thống phải cho phép khách hàng theo dõi trạng thái chuyến và vị trí tài xế theo thời gian thực.

Thông tin hiển thị:

Vị trí hiện tại của tài xế.
Trạng thái chuyến.
Thông tin tài xế.
Điểm đón.
Điểm đến.
BG-06 - Cập nhật trạng thái chuyến

Mô tả:
Hệ thống phải cho phép tài xế cập nhật trạng thái chuyến trong quá trình thực hiện.

Các trạng thái chính:

DA_NHAN_CHUYEN
      ↓
DA_DEN_DIEM_DON
      ↓
DA_DON_KHACH
      ↓
DANG_DI_CHUYEN
      ↓
HOAN_THANH


Ngoài ra, chuyến có thể chuyển sang:

HUY_CHUYEN


khi khách hàng hoặc tài xế thực hiện hủy chuyến theo quy định.

BG-07 - Tính cước chuyến

Mô tả:
Hệ thống phải tự động tính cước chuyến sau khi chuyến hoàn thành.

Kết quả:

Xác định số tiền khách hàng cần thanh toán.
Tạo thông tin thanh toán tương ứng với chuyến đi.
BG-08 - Thanh toán chuyến đi

Mô tả:
Hệ thống phải cho phép khách hàng thực hiện thanh toán và lưu lại trạng thái giao dịch.

Trạng thái giao dịch:

PENDING
   ↓
SUCCESS


hoặc:

PENDING
   ↓
FAILED


Hệ thống không lưu thông tin thẻ nhạy cảm mà chỉ lưu thông tin cần thiết để quản lý giao dịch.

BG-09 - Lịch sử chuyến đi

Mô tả:
Hệ thống phải cho phép khách hàng xem lại lịch sử các chuyến đã thực hiện.

Thông tin bao gồm:

Mã chuyến.
Thời gian.
Điểm đón.
Điểm đến.
Tài xế.
Trạng thái chuyến.
Số tiền thanh toán.
Trạng thái thanh toán.
BG-10 - Quản lý và giám sát vận hành

Mô tả:
Hệ thống phải cho phép Admin/Operator quản lý thông tin khách hàng, tài xế và giám sát các chuyến đi.

Chức năng:

Xem danh sách khách hàng.
Xem danh sách tài xế.
Xem trạng thái tài xế.
Xem danh sách chuyến.
Xem chi tiết chuyến.
Hỗ trợ xử lý chuyến gặp sự cố.
BG-11 - Báo cáo vận hành

Mô tả:
Hệ thống phải cho phép Admin/Operator xem các báo cáo cơ bản phục vụ hoạt động kinh doanh.

Báo cáo gồm:

Tổng số chuyến.
Số chuyến hoàn thành.
Số chuyến hủy.
Tổng doanh thu.
Số khách hàng.
Số tài xế.
BG-12 - Đảm bảo hoạt động liên tục

Mô tả:
Hệ thống phải đảm bảo luồng đặt xe không bị gián đoạn hoàn toàn khi các dịch vụ phụ trợ như thanh toán hoặc thông báo gặp lỗi tạm thời.

Yêu cầu:

Ghi nhận trạng thái lỗi của dịch vụ.
Không làm mất thông tin chuyến đi.
Cho phép xử lý lại giao dịch khi cần.
Các module chính tiếp tục hoạt động độc lập khi có lỗi ở module phụ trợ.
