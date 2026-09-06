# SOFTWARE REQUIREMENTS SPECIFICATION (SRS)

**Dự án:** Hệ thống Đặt xe Trực tuyến (CAB System)
**Tác giả:** Lê Phúc Trung
**Mã số sinh viên:** 23703751
**Thời gian thực hiện:** 7 tuần

---

# 1. GIỚI THIỆU (INTRODUCTION)

## 1.1. Mục đích

Tài liệu Đặc tả Yêu cầu Phần mềm (SRS) này mô tả chi tiết các yêu cầu chức năng, yêu cầu phi chức năng, quy tắc nghiệp vụ và kiến trúc tổng quan cho dự án CAB System của Công ty ABC. Tài liệu đóng vai trò làm cơ sở để nhóm phát triển, kiểm thử (QA/QC) và các bên liên quan (Stakeholders) triển khai sản phẩm đúng tiến độ 7 tuần.

## 1.2. Bối cảnh & Báo cáo bài toán

Công ty ABC hiện kinh doanh dịch vụ đặt xe trực tuyến nhưng gặp nhiều hạn chế:

* Phân công tài xế chủ yếu thực hiện thủ công.
* Khách hàng khó theo dõi trạng thái chuyến đi theo thời gian thực.
* Thông tin thanh toán chưa được quản lý tập trung.
* Khó mở rộng hệ thống khi số lượng chuyến đi tăng cao.

## 1.3. Mục tiêu dự án

* Xây dựng nền tảng CAB hiện đại, linh hoạt, phục vụ quy mô lớn khách hàng và tài xế.
* Tự động hóa quy trình ghép xe (Matching Engine), thanh toán và gửi thông báo.
* Đảm bảo kiến trúc có khả năng mở rộng độc lập, cho phép bổ sung dịch vụ/phương thức thanh toán mới trong tương lai.

---

# 2. BÊN LIÊN QUAN HỆ THỐNG (STAKEHOLDERS)

| Stakeholder                             | Vai trò                                                                                                       |
| --------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| **Khách hàng (Customer)**               | Người sử dụng dịch vụ, tạo yêu cầu đặt xe, theo dõi chuyến đi, thực hiện thanh toán và xem lịch sử chuyến đi. |
| **Tài xế (Driver)**                     | Cung cấp dịch vụ vận chuyển, nhận hoặc từ chối chuyến, cập nhật trạng thái chuyến và cung cấp vị trí GPS.     |
| **Nhân viên vận hành (Admin/Operator)** | Quản lý khách hàng, tài xế, chuyến đi, hỗ trợ xử lý sự cố và theo dõi hoạt động của hệ thống.                 |
| **Ban Giám Đốc**                        | Sponsor dự án, định hướng phát triển hệ thống, theo dõi tiến độ triển khai và hiệu quả hoạt động kinh doanh.  |
| **Bộ phận Kế toán**                     | Theo dõi doanh thu, quản lý và đối soát các giao dịch thanh toán.                                             |


---

# 3. QUY TRÌNH NGHIỆP VỤ CỐT LÕI (CORE BUSINESS PROCESS)

Quy trình đặt xe và thực hiện chuyến đi của CAB System:

| Bước | Trạng thái          | Mô tả                    |
| ---: | ------------------- | ------------------------ |
|    1 | KHOI_TAO            | Khách hàng đặt xe.       |
|    2 | TIM_TAI_XE          | Hệ thống tìm tài xế.     |
|    3 | CHO_TAI_XE_XAC_NHAN | Gửi đề xuất đến tài xế.  |
|    4 | DA_NHAN_CHUYEN      | Tài xế nhận chuyến.      |
|    5 | DA_DEN_DIEM_DON     | Tài xế đến điểm đón.     |
|    6 | DA_DON_KHACH        | Khách lên xe.            |
|    7 | DANG_DI_CHUYEN      | Bắt đầu di chuyển.       |
|    8 | HOAN_THANH          | Đến điểm đến.            |
|    9 | THANH_TOAN          | Tính cước và thanh toán. |
|   10 | HOÀN TẤT            | Hoàn tất và đánh giá.    |

**Các trường hợp đặc biệt:**

| Trường hợp            | Xử lý                                   |
| --------------------- | --------------------------------------- |
| Tài xế từ chối        | Quay lại TIM_TAI_XE để tìm tài xế khác. |
| Tài xế Timeout        | Quay lại TIM_TAI_XE và thực hiện Retry. |
| Không tìm thấy tài xế | Chuyển sang HUY_CHUYEN.                 |
| Khách / Tài xế hủy    | Chuyển sang HUY_CHUYEN.                 |

---

```mermaid
flowchart TD
    A([Bắt đầu]) --> B[Khách hàng nhập điểm đón và điểm đến]
    B --> C{Thông tin hợp lệ?}

    C -- Không --> B
    C -- Có --> D[Tạo yêu cầu chuyến đi]
    D --> E[Trạng thái: TIM_TAI_XE]

    E --> F[Hệ thống tìm tài xế phù hợp]
    F --> G[Gửi đề xuất chuyến đến tài xế]

    G --> H{Tài xế xử lý}

    H -- Accept --> I[Chuyến được xác nhận]
    H -- Reject --> F
    H -- Timeout --> F

    I --> J[Trạng thái: DA_NHAN_CHUYEN]
    J --> K[Tài xế đến điểm đón]
    K --> L[Trạng thái: DA_DEN_DIEM_DON]
    L --> M[Tài xế đón khách]
    M --> N[Trạng thái: DA_DON_KHACH]
    N --> O[Thực hiện chuyến]
    O --> P[Trạng thái: DANG_DI_CHUYEN]
    P --> Q[Hoàn thành chuyến]
    Q --> R[Trạng thái: HOAN_THANH]
```

# 4. PHẠM VI MODULE HỆ THỐNG (SYSTEM MODULES)

Để đảm bảo dự án CAB System có thể hoàn thành trong thời gian 7 tuần, hệ thống được giới hạn trong 5 module chính.

## 4.1. Module 1 - Quản lý Khách hàng (Customer Management)

**Mục đích:**
Quản lý tài khoản và thông tin cơ bản của khách hàng, phục vụ quá trình đặt xe và sử dụng dịch vụ.

**Business Goal:** BG-02, BG-04

| STT | Chức năng MVP                |
| --: | ---------------------------- |
|   1 | Đăng ký tài khoản            |
|   2 | Đăng nhập / đăng xuất        |
|   3 | Xem thông tin cá nhân        |
|   4 | Cập nhật thông tin cá nhân   |
|   5 | Quản lý trạng thái tài khoản |
|   6 | Xem lịch sử chuyến đi        |

## 4.2. Module 2 - Quản lý Tài xế (Driver Management)

**Mục đích:**
Quản lý thông tin tài xế và trạng thái hoạt động, làm cơ sở cho việc tự động ghép tài xế với khách hàng.

**Business Goal:** BG-01, BG-04, BG-05

| STT | Chức năng MVP                        |
| --: | ------------------------------------ |
|   1 | Đăng nhập tài khoản tài xế           |
|   2 | Xem / cập nhật thông tin tài xế      |
|   3 | Cập nhật trạng thái Online / Offline |
|   4 | Cập nhật trạng thái Available / Busy |
|   5 | Cập nhật vị trí GPS                  |
|   6 | Nhận đề xuất chuyến                  |
|   7 | Chấp nhận chuyến                     |
|   8 | Từ chối chuyến                       |
|   9 | Xem lịch sử chuyến                   |

## 4.3. Module 3 - Đặt xe & Ghép tài xế (Booking & Matching)

**Mục đích:**
Là module nghiệp vụ cốt lõi của CAB System, chịu trách nhiệm tiếp nhận yêu cầu đặt xe và tự động tìm tài xế phù hợp.

**Business Goal:** BG-01, BG-02, BG-05

| STT | Chức năng MVP                    |
| --: | -------------------------------- |
|   1 | Nhập điểm đón                    |
|   2 | Nhập điểm đến                    |
|   3 | Tạo yêu cầu đặt xe               |
|   4 | Tính giá dự kiến                 |
|   5 | Tìm tài xế phù hợp               |
|   6 | Gửi đề xuất chuyến cho tài xế    |
|   7 | Xử lý tài xế chấp nhận / từ chối |
|   8 | Xử lý Timeout                    |
|   9 | Retry tìm tài xế                 |
|  10 | Xác nhận tài xế                  |
|  11 | Hủy chuyến                       |
|  12 | Quản lý trạng thái chuyến        |

### Trạng thái chuyến

| STT | Trạng thái          |
| --: | ------------------- |
|   1 | KHOI_TAO            |
|   2 | TIM_TAI_XE          |
|   3 | CHO_TAI_XE_XAC_NHAN |
|   4 | DA_NHAN_CHUYEN      |
|   5 | DA_DEN_DIEM_DON     |
|   6 | DA_DON_KHACH        |
|   7 | DANG_DI_CHUYEN      |
|   8 | HOAN_THANH          |
|   9 | HUY_CHUYEN          |

## 4.4. Module 4 - Theo dõi Chuyến & Thanh toán (Ride Tracking & Payment)

**Mục đích:**
Quản lý quá trình thực hiện chuyến đi, cập nhật vị trí tài xế và xử lý thanh toán sau khi chuyến hoàn thành.

**Business Goal:** BG-02, BG-03, BG-05

| Nhóm            | STT | Chức năng MVP                         |
| --------------- | --: | ------------------------------------- |
| Theo dõi chuyến |   1 | Theo dõi chuyến                       |
| Theo dõi chuyến |   2 | Cập nhật vị trí GPS tài xế            |
| Theo dõi chuyến |   3 | Hiển thị vị trí tài xế cho khách hàng |
| Theo dõi chuyến |   4 | Cập nhật trạng thái chuyến            |
| Theo dõi chuyến |   5 | Tài xế xác nhận đã đến điểm đón       |
| Theo dõi chuyến |   6 | Tài xế xác nhận đã đón khách          |
| Theo dõi chuyến |   7 | Tài xế bắt đầu chuyến                 |
| Theo dõi chuyến |   8 | Tài xế hoàn thành chuyến              |
| Thanh toán      |   9 | Tính cước chuyến                      |
| Thanh toán      |  10 | Chọn phương thức thanh toán           |
| Thanh toán      |  11 | Tạo giao dịch                         |
| Thanh toán      |  12 | Gửi giao dịch đến Payment Provider    |
| Thanh toán      |  13 | Nhận kết quả thanh toán               |
| Thanh toán      |  14 | Lưu trạng thái giao dịch              |
| Thanh toán      |  15 | Xử lý thanh toán thất bại             |

## 4.5. Module 5 - Quản trị & Vận hành (Admin & Operation)

**Mục đích:**
Cung cấp công cụ cho nhân viên vận hành quản lý khách hàng, tài xế và giám sát chuyến đi.

**Business Goal:** BG-03, BG-04, BG-05

| STT | Chức năng MVP                              |
| --: | ------------------------------------------ |
|   1 | Đăng nhập Admin / Operator                 |
|   2 | Quản lý khách hàng                         |
|   3 | Quản lý tài xế                             |
|   4 | Xem trạng thái Online / Offline của tài xế |
|   5 | Xem danh sách chuyến đi                    |
|   6 | Xem chi tiết chuyến đi                     |
|   7 | Theo dõi chuyến đang hoạt động             |
|   8 | Hỗ trợ xử lý chuyến lỗi                    |
|   9 | Xem giao dịch thanh toán                   |
|  10 | Xem báo cáo cơ bản                         |

## 4.6. Tổng quan phạm vi Module

| Module              | Vai trò chính         |
| ------------------- | --------------------- |
| Customer Management | Quản lý khách hàng    |
| Driver Management   | Quản lý tài xế        |
| Booking & Matching  | Đặt xe và ghép tài xế |
| Ride Tracking       | Theo dõi chuyến       |
| Payment             | Thanh toán            |
| Admin & Operation   | Quản trị và vận hành  |

**Giới hạn MVP:**
Không triển khai các chức năng nâng cao như Loyalty, Voucher nâng cao, Carpooling, Dynamic Pricing phức tạp, AI/ML, BI nâng cao và quản lý tài chính chuyên sâu.

---

# 5. BUSINESS REQUIREMENTS (BR)

Phần này mô tả các yêu cầu nghiệp vụ mà hệ thống CAB System phải đáp ứng để hỗ trợ quy trình đặt xe, quản lý tài xế, theo dõi chuyến đi, thanh toán và vận hành hệ thống.

## 5.1. Danh sách Business Requirements

| Mã    | Business Requirement                                                                                                                          | Module                           |
| ----- | --------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------- |
| BG-01 | Hệ thống phải cho phép khách hàng tạo yêu cầu chuyến đi bằng cách nhập điểm đón và điểm đến.                                                  | Quản lý Khách hàng / Đặt xe      |
| BG-02 | Hệ thống phải cho phép khách hàng lựa chọn tài xế được hệ thống đề xuất và xác nhận chuyến đi.                                                | Đặt xe & Ghép tài xế             |
| BG-03 | Hệ thống phải tự động tìm kiếm và đề xuất tài xế phù hợp dựa trên trạng thái hoạt động và vị trí hiện tại.                                    | Quản lý Tài xế / Matching        |
| BG-04 | Hệ thống phải cho phép tài xế nhận hoặc từ chối yêu cầu chuyến đi trong một khoảng thời gian xác định.                                        | Quản lý Tài xế                   |
| BG-05 | Hệ thống phải cho phép khách hàng theo dõi trạng thái chuyến đi và vị trí tài xế trong quá trình thực hiện chuyến.                            | Theo dõi Chuyến                  |
| BG-06 | Hệ thống phải cho phép tài xế cập nhật trạng thái chuyến từ khi nhận chuyến đến khi hoàn thành hoặc hủy chuyến.                               | Quản lý Tài xế / Theo dõi Chuyến |
| BG-07 | Hệ thống phải tự động tính cước chuyến dựa trên thông tin chuyến đi và cung cấp số tiền cần thanh toán cho khách hàng.                        | Thanh toán                       |
| BG-08 | Hệ thống phải cho phép khách hàng thực hiện thanh toán và lưu lại trạng thái của giao dịch.                                                   | Thanh toán                       |
| BG-09 | Hệ thống phải cho phép khách hàng xem lịch sử các chuyến đi và thông tin thanh toán tương ứng.                                                | Quản lý Khách hàng               |
| BG-10 | Hệ thống phải cho phép nhân viên vận hành quản lý khách hàng, tài xế và theo dõi các chuyến đi đang hoạt động.                                | Quản trị & Vận hành              |
| BG-11 | Hệ thống phải cho phép nhân viên vận hành xem thông tin giao dịch và báo cáo cơ bản về số chuyến, chuyến hoàn thành, chuyến hủy và doanh thu. | Quản trị & Vận hành              |
| BG-12 | Hệ thống phải đảm bảo các chức năng đặt xe vẫn hoạt động khi dịch vụ thanh toán hoặc thông báo gặp sự cố tạm thời.                            | Toàn hệ thống                    |

---

# 5.2. CHI TIẾT BUSINESS REQUIREMENTS

## BG-01 - Tạo yêu cầu chuyến đi

**Mô tả:**
Hệ thống phải cho phép khách hàng tạo yêu cầu chuyến đi bằng cách nhập thông tin điểm đón và điểm đến.

| Loại      | Nội dung                                       |
| --------- | ---------------------------------------------- |
| Điều kiện | Khách hàng đã đăng nhập                        |
| Điều kiện | Điểm đón hợp lệ                                |
| Điều kiện | Điểm đến hợp lệ                                |
| Kết quả   | Hệ thống tạo một yêu cầu chuyến đi             |
| Kết quả   | Yêu cầu được chuyển sang trạng thái TIM_TAI_XE |

## BG-02 - Lựa chọn và xác nhận tài xế

**Mô tả:**
Hệ thống phải cho phép khách hàng lựa chọn tài xế được hệ thống đề xuất và xác nhận chuyến đi.

| Kết quả                                      |
| -------------------------------------------- |
| Tài xế được gán vào chuyến                   |
| Chuyến chuyển sang trạng thái DA_NHAN_CHUYEN |

## BG-03 - Tự động ghép tài xế

**Mô tả:**
Hệ thống phải tự động tìm kiếm và đề xuất tài xế phù hợp cho yêu cầu chuyến đi.

| STT | Tiêu chí lựa chọn                   |
| --: | ----------------------------------- |
|   1 | Tài xế đang Online                  |
|   2 | Tài xế đang Available               |
|   3 | Tài xế có vị trí GPS hợp lệ         |
|   4 | Tài xế phù hợp với khu vực điểm đón |

**Kết quả:**

| STT | Kết quả                                                            |
| --: | ------------------------------------------------------------------ |
|   1 | Hệ thống gửi đề xuất chuyến đến tài xế                             |
|   2 | Nếu tài xế từ chối hoặc Timeout, hệ thống tiếp tục tìm tài xế khác |

## BG-04 - Tài xế nhận hoặc từ chối chuyến

**Mô tả:**
Hệ thống phải cho phép tài xế nhận hoặc từ chối yêu cầu chuyến đi trong thời gian quy định.

| Hành động | Kết quả                                                      |
| --------- | ------------------------------------------------------------ |
| Accept    | Chuyến được xác nhận                                         |
| Reject    | Hệ thống tìm tài xế khác                                     |
| Timeout   | Hệ thống xem như tài xế không nhận chuyến và thực hiện Retry |

## BG-05 - Theo dõi chuyến đi

**Mô tả:**
Hệ thống phải cho phép khách hàng theo dõi trạng thái chuyến và vị trí tài xế theo thời gian thực.

| STT | Thông tin hiển thị         |
| --: | -------------------------- |
|   1 | Vị trí hiện tại của tài xế |
|   2 | Trạng thái chuyến          |
|   3 | Thông tin tài xế           |
|   4 | Điểm đón                   |
|   5 | Điểm đến                   |

## BG-06 - Cập nhật trạng thái chuyến

**Mô tả:**
Hệ thống phải cho phép tài xế cập nhật trạng thái chuyến trong quá trình thực hiện.

| STT | Trạng thái      |
| --: | --------------- |
|   1 | DA_NHAN_CHUYEN  |
|   2 | DA_DEN_DIEM_DON |
|   3 | DA_DON_KHACH    |
|   4 | DANG_DI_CHUYEN  |
|   5 | HOAN_THANH      |
|   6 | HUY_CHUYEN      |

**Điều kiện hủy:**
Chuyến có thể chuyển sang HUY_CHUYEN khi khách hàng hoặc tài xế thực hiện hủy chuyến theo quy định.



## BG-08 - Thanh toán chuyến đi

**Mô tả:**
Hệ thống phải cho phép khách hàng thực hiện thanh toán và lưu lại trạng thái giao dịch.

| Trạng thái | Ý nghĩa                  |
| ---------- | ------------------------ |
| PENDING    | Giao dịch đang chờ xử lý |
| SUCCESS    | Thanh toán thành công    |
| FAILED     | Thanh toán thất bại      |

Hệ thống không lưu thông tin thẻ nhạy cảm mà chỉ lưu thông tin cần thiết để quản lý giao dịch.

## BG-09 - Lịch sử chuyến đi

**Mô tả:**
Hệ thống phải cho phép khách hàng xem lại lịch sử các chuyến đã thực hiện.

| STT | Thông tin             |
| --: | --------------------- |
|   1 | Mã chuyến             |
|   2 | Thời gian             |
|   3 | Điểm đón              |
|   4 | Điểm đến              |
|   5 | Tài xế                |
|   6 | Trạng thái chuyến     |
|   7 | Số tiền thanh toán    |
|   8 | Trạng thái thanh toán |

# 6. MÔ HÌNH HÓA NGHIỆP VỤ (BUSINESS PROCESS MODELING)

## 6.1. Tổng quan các quy trình nghiệp vụ

Dựa trên các Business Requirements từ BG-01 đến BG-09, hệ thống CAB có thể được mô hình hóa thành các quy trình nghiệp vụ chính sau:

| STT | Quy trình nghiệp vụ             | Business Requirement liên quan |
| --: | ------------------------------- | ------------------------------ |
|   1 | Tạo yêu cầu chuyến đi           | BG-01                          |
|   2 | Lựa chọn và xác nhận tài xế     | BG-02                          |
|   3 | Tự động ghép tài xế             | BG-03                          |
|   4 | Tài xế nhận hoặc từ chối chuyến | BG-04                          |
|   5 | Theo dõi chuyến đi              | BG-05                          |
|   6 | Cập nhật trạng thái chuyến      | BG-06                          |
|   7 | Thanh toán chuyến đi            | BG-08                          |
|   8 | Xem lịch sử chuyến đi           | BG-09                          |

> **Lưu ý:** Nội dung BG-07 không được cung cấp trong phần Business Requirements trên, nên không đưa vào mô hình chi tiết để tránh tự bổ sung thông tin ngoài tài liệu.

---

## 6.2. Quy trình tạo yêu cầu chuyến đi

**Mục đích:** Cho phép khách hàng tạo một yêu cầu chuyến đi bằng cách cung cấp điểm đón và điểm đến.

| Thành phần           | Nội dung                                               |
| -------------------- | ------------------------------------------------------ |
| Actor chính          | Khách hàng                                             |
| Điều kiện trước      | Khách hàng đã đăng nhập                                |
| Dữ liệu đầu vào      | Điểm đón, điểm đến                                     |
| Xử lý                | Hệ thống kiểm tra tính hợp lệ của điểm đón và điểm đến |
| Kết quả              | Tạo yêu cầu chuyến đi                                  |
| Trạng thái tiếp theo | TIM_TAI_XE                                             |

**Luồng nghiệp vụ:**

| Bước | Hoạt động                                      |
| ---: | ---------------------------------------------- |
|    1 | Khách hàng đăng nhập hệ thống                  |
|    2 | Khách hàng nhập điểm đón                       |
|    3 | Khách hàng nhập điểm đến                       |
|    4 | Hệ thống kiểm tra tính hợp lệ của thông tin    |
|    5 | Hệ thống tạo yêu cầu chuyến đi                 |
|    6 | Yêu cầu được chuyển sang trạng thái TIM_TAI_XE |

---

## 6.3. Quy trình tự động ghép và xác nhận tài xế

**Mục đích:** Tìm kiếm tài xế phù hợp và thực hiện xác nhận chuyến đi.

### 6.3.1. Tiêu chí tìm kiếm tài xế

| STT | Tiêu chí                            |
| --: | ----------------------------------- |
|   1 | Tài xế đang Online                  |
|   2 | Tài xế đang Available               |
|   3 | Tài xế có vị trí GPS hợp lệ         |
|   4 | Tài xế phù hợp với khu vực điểm đón |

### 6.3.2. Luồng nghiệp vụ

| Bước | Hoạt động                                                                 |
| ---: | ------------------------------------------------------------------------- |
|    1 | Hệ thống nhận yêu cầu chuyến ở trạng thái TIM_TAI_XE                      |
|    2 | Hệ thống tìm kiếm tài xế phù hợp theo các tiêu chí                        |
|    3 | Hệ thống gửi đề xuất chuyến đến tài xế                                    |
|    4 | Tài xế lựa chọn Accept hoặc Reject                                        |
|    5 | Nếu Accept, chuyến được xác nhận                                          |
|    6 | Nếu Reject, hệ thống tìm tài xế khác                                      |
|    7 | Nếu Timeout, hệ thống xem tài xế như không nhận chuyến và thực hiện Retry |
|    8 | Khi tài xế được xác nhận, chuyến chuyển sang trạng thái DA_NHAN_CHUYEN    |

---

## 6.4. Quy trình theo dõi và thực hiện chuyến đi

**Mục đích:** Cho phép khách hàng theo dõi thông tin chuyến đi và vị trí tài xế theo thời gian thực.

### 6.4.1. Thông tin được hiển thị

| STT | Thông tin                  |
| --: | -------------------------- |
|   1 | Vị trí hiện tại của tài xế |
|   2 | Trạng thái chuyến          |
|   3 | Thông tin tài xế           |
|   4 | Điểm đón                   |
|   5 | Điểm đến                   |

### 6.4.2. Luồng cập nhật trạng thái

| STT | Trạng thái      |
| --: | --------------- |
|   1 | DA_NHAN_CHUYEN  |
|   2 | DA_DEN_DIEM_DON |
|   3 | DA_DON_KHACH    |
|   4 | DANG_DI_CHUYEN  |
|   5 | HOAN_THANH      |
|   6 | HUY_CHUYEN      |

**Luồng nghiệp vụ:**

| Bước | Hoạt động                                                                                                    |
| ---: | ------------------------------------------------------------------------------------------------------------ |
|    1 | Tài xế nhận chuyến                                                                                           |
|    2 | Tài xế di chuyển đến điểm đón                                                                                |
|    3 | Tài xế cập nhật trạng thái DA_DEN_DIEM_DON                                                                   |
|    4 | Tài xế đón khách và cập nhật DA_DON_KHACH                                                                    |
|    5 | Tài xế thực hiện chuyến và cập nhật DANG_DI_CHUYEN                                                           |
|    6 | Khi đến điểm đến, tài xế cập nhật HOAN_THANH                                                                 |
|    7 | Trong quá trình thực hiện, chuyến có thể chuyển sang HUY_CHUYEN khi khách hàng hoặc tài xế hủy theo quy định |
|    8 | Khách hàng theo dõi trạng thái và vị trí tài xế trên hệ thống                                                |

---

## 6.5. Quy trình thanh toán chuyến đi

**Mục đích:** Cho phép khách hàng thanh toán và hệ thống quản lý trạng thái giao dịch.

| Thành phần         | Nội dung                                            |
| ------------------ | --------------------------------------------------- |
| Actor chính        | Khách hàng                                          |
| Đầu vào            | Yêu cầu thanh toán chuyến đi                        |
| Xử lý              | Hệ thống thực hiện và cập nhật trạng thái giao dịch |
| Kết quả            | Lưu trạng thái giao dịch                            |
| Thông tin nhạy cảm | Hệ thống không lưu thông tin thẻ nhạy cảm           |

### Trạng thái giao dịch

| Trạng thái | Ý nghĩa                  |
| ---------- | ------------------------ |
| PENDING    | Giao dịch đang chờ xử lý |
| SUCCESS    | Thanh toán thành công    |
| FAILED     | Thanh toán thất bại      |

### Luồng nghiệp vụ

| Bước | Hoạt động                                                 |
| ---: | --------------------------------------------------------- |
|    1 | Khách hàng thực hiện thanh toán                           |
|    2 | Hệ thống tạo/xử lý giao dịch                              |
|    3 | Giao dịch ở trạng thái PENDING                            |
|    4 | Nếu thanh toán thành công, trạng thái chuyển sang SUCCESS |
|    5 | Nếu thanh toán thất bại, trạng thái chuyển sang FAILED    |
|    6 | Hệ thống lưu thông tin cần thiết để quản lý giao dịch     |

---

## 6.6. Quy trình xem lịch sử chuyến đi

**Mục đích:** Cho phép khách hàng xem lại các chuyến đi đã thực hiện và thông tin thanh toán liên quan.

### Thông tin lịch sử

| STT | Thông tin             |
| --: | --------------------- |
|   1 | Mã chuyến             |
|   2 | Thời gian             |
|   3 | Điểm đón              |
|   4 | Điểm đến              |
|   5 | Tài xế                |
|   6 | Trạng thái chuyến     |
|   7 | Số tiền thanh toán    |
|   8 | Trạng thái thanh toán |

### Luồng nghiệp vụ

| Bước | Hoạt động                                                |
| ---: | -------------------------------------------------------- |
|    1 | Khách hàng truy cập chức năng lịch sử chuyến đi          |
|    2 | Hệ thống lấy danh sách các chuyến đã thực hiện           |
|    3 | Hệ thống hiển thị thông tin từng chuyến                  |
|    4 | Khách hàng xem thông tin chuyến và trạng thái thanh toán |

---

## 6.7. Tổng hợp luồng nghiệp vụ chính

| Bước | Quy trình                       | Trạng thái / Kết quả             |
| ---: | ------------------------------- | -------------------------------- |
|    1 | Khách hàng tạo yêu cầu chuyến   | TIM_TAI_XE                       |
|    2 | Hệ thống tìm tài xế phù hợp     | Tài xế được đề xuất              |
|    3 | Tài xế nhận chuyến              | DA_NHAN_CHUYEN                   |
|    4 | Tài xế đến điểm đón             | DA_DEN_DIEM_DON                  |
|    5 | Tài xế đón khách                | DA_DON_KHACH                     |
|    6 | Tài xế thực hiện chuyến         | DANG_DI_CHUYEN                   |
|    7 | Chuyến hoàn thành               | HOAN_THANH                       |
|    8 | Khách hàng thực hiện thanh toán | PENDING / SUCCESS / FAILED       |
|    9 | Hệ thống lưu thông tin chuyến   | Hiển thị trong lịch sử chuyến đi |

### Luồng xử lý khi tài xế không nhận chuyến

| Tình huống                 | Xử lý                                       |
| -------------------------- | ------------------------------------------- |
| Tài xế Accept              | Chuyến được xác nhận                        |
| Tài xế Reject              | Hệ thống tìm tài xế khác                    |
| Tài xế Timeout             | Hệ thống thực hiện Retry và tìm tài xế khác |
| Không có tài xế phù hợp    | Tiếp tục tìm kiếm theo cơ chế của hệ thống  |
| Khách hàng hoặc tài xế hủy | Chuyến chuyển sang HUY_CHUYEN               |



