# DML Trigger
## Giới thiệu
- kiểm soát sự thay đổi dữ liệu
- gắn liền với một bảng nào đó
### Ý nghĩa
- bảo đảm toàn vẹn dữ liệu
- sử dụng dml trigger một cahcs hợp lý sẽ có tác động rất lớn trong việc tăng hiệu năng của csdl
## Tạo trigger
- cú pháp
```sql
CREATE TRIGGER trigger_name
ON table_name
AFTER|INSTEAD OF
INSERT|UPDATE|DELETE
AS
BEGIN
    -- các câu lệnh xử lý
END
```
- khi câu lệnh delete được thực thi trên bảng, các dòng dữ liệu bị xóa sẽ được sao chép vào bảng deleted. bảng inserted trong trường hợp naỳ không có dữ liệu
- bảng inserted chứa các dòng dữ liệu mới được thêm vào hoặc cập nhật
- bảng deleted chứa các dòng dữ liệu bị xóa hoặc bị cập nhật
## lệnh rollback transaction
- lệnh rollback transaction sẽ hủy bỏ tất cả các thay đổi trong giao dịch hiện tại
- lệnh rollback transaction sẽ không hủy bỏ các thay đổi trong các giao dịch trước đó

# DDL Trigger

## giới thiệu
- kích hoạt khi người dùng thay đổi cấu trúc csdl hay đối tượng csdl bằng các phát biểu sql thuộc ddl như: create,alter,drop,grant,deny,revoke
- nếu dml trigger dùng để kiểm soát dữ liệu chứa trong table hay view thì đl trigger có thể sử dụng cho chức năng
- sau khi thiết kế csdl hoàn tất, để kiểm soát mọi thay đổi cấu trúc của csdl thì dùng loại trigger này
- mục đích: kiểm soát mọi sự thay đổi cấu trúc
## Tạo trigger
- cú pháp
```sql
CREATE TRIGGER trigger_name
ON ALL SERVER
FOR CREATE|ALTER|DROP
AS
BEGIN
    -- các câu lệnh xử lý
END
```
## hàm eventdataa
- thông tin về những sự kiện làm kích hoạt ddl trigger được lưu lại trong hàm eventdata
