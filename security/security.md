# chứng thực người dùng
- authentication đảm bảo rằng người dùng là ai
## 2 kiểu chứng thực:
    - chứng thực SQL Server: người dùng được xác thực bởi SQL Server
    - chứng thực Windows: người dùng được xác thực bởi Windows
- mặc định sql express, developer không cho các kết nối từ xa
- để cấu hình, phải chấp nhận các kết nối từ xa cần thực hiện các bước sau:
    + cho phép tiếp nhận các kết nối từ xa
## đăng nhập
- roles trong sql server tương đương group trong windows
- tạo nhóm, sau đó cấp quan hệ thành viên cho nhóm
### 4 nhóm:
- quyền server: được xây dựng sẵn trong sql server và người dùng không thể thay đổi thêm xóa
- quyền csdl
- nhóm quyền csdl do người dungf định nghĩa:
- nhóm quyền ứng dụng

### thêm người dùng vào nhóm quyền server, csdl, tạo nhóm quyền do người dùng tự định nghĩa

# gán quyền cho người dùng
## tạo người dùng csdl
## quản lý quyền trên đối tượng

# bảo trì csdl
## sao lưu dự phòng
## khôi phục csdl

