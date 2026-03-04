# ADR-001: Lựa chọn kiến trúc Layered
## Trạng thái
Accepted

## Bối cảnh
Chúng tôi cần xây dựng hệ thống quản lý công việc theo dạng Kanban board (Trello Clone) với các tính năng: quản lý workspace, board, task, gán thành viên, bình luận và thống kê.

Hệ thống có quy mô vừa, team 3 người, thời gian phát triển 6 tuần.

Đối tượng sử dụng (Actors): Thành viên, Quản lý dự án, Admin.

## Quyết định
Sử dụng kiến trúc Layered (Phân tầng) 4 tầng để đảm bảo tính tổ chức và dễ dàng mở rộng các tính năng quản lý:

- Presentation Layer:
    - React JS: Xử lý hiển thị và tương tác người dùng.
- API & Security Layer (Giao tiếp & Bảo mật)
    - Spring MVC: Đóng vai trò bộ khung điều hướng (Controller), tiếp nhận request và trả về dữ liệu chuẩn RESTful API.
    - Spring Security & JWT: Hệ thống bảo mật cấp "thẻ thông hành" (Token) để xác thực và phân quyền người dùng sau khi đăng nhập.
- Business Logic Layer:
    - Spring Service: Trung tâm xử lý logic lõi của ứng dụng (như tính toán tiến độ, kiểm tra hạn chót, xử lý quy trình di chuyển Task giữa các cột).
- Data Access Layer (Spring Data JPA / Hibernate)
    - Spring Data JPA (Hibernate): Thay vì dùng JDBC thủ công, JPA giúp ánh xạ trực tiếp các bảng MySQL thành đối tượng Java, giúp thao tác dữ liệu cực nhanh qua các hàm có sẵn.

## Lý do
1. Dễ hiểu và triển khai cho team
2. Tách biệt trách nhiệm (Separation of Concerns)
3. Dễ bảo trì và test
4. Phù hợp với quy mô dự án
## Hệ quả
Tích cực: Dễ bảo trì và viết unit test cho từng tầng riêng biệt. Cấu trúc rõ ràng giúp việc bàn giao hoặc thêm thành viên mới vào team thuận tiện.

Tiêu cực: Với các tác vụ cực kỳ đơn giản (như lấy tên một nhãn màu), việc phải đi qua cả 3 tầng có thể làm tăng thời gian viết code ban đầu.

## Ngày quyết định
2026-03-03