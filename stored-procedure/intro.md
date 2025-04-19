# Thủ tục nội tại (stored procedure)
## 3.1 Giới thiệu thủ tục
- Giảm thiểu sự lưu thông trên mạng: Khi cần thực hiện một lượng lớn câu lệnh T-SQL(SQL mở rộng), thủ tục nội tại thực hiện nhanh hơn vì khi máy chủ nhận được nhiều câu lệnh cùng một lúc đều phải kiểm tra tính hợp lệ quyền của tài khoản từ máy khách và các tham số khác. Khi thủ tục cần gọi nhiều lần trên các máy khách thì thủ tục thực hiện một lần đầu tiên, những lần sau máy khách sẽ chạy thủ tục đã được biên dịch
- bảo mật tốt hơn
## 3.2 Phân loại thủ tục
3 nhóm 
- Nhóm 1: do người dùng tạo ra. GỒm 2 loại:
    - loại thủ tục nội tại được người dùng tạo ra và lưu vào csdl. chúng chứa các phát biểu t-sql
    - loại thứ 2 được khai báo và tạo ra bằng ngôn ngữ lập trình .net
- nhóm 2: thủ tục nội tại hệ htoongs thực hiện các chức năng quản trị csdl thường dùng. các thủ tục này chứa trong csdl resource
    - danh sách các thủ tục nội tại hệ thống hiển thị trong ngăn system stored procedure
    - thủ tục nội tại trong csdl resouce luôn có tên với tiền tốt là sp_ --> không nên đặt tên thủ tục nội tại do mình tạo ra bằng tiền tố này
- nhóm 3: thủ tục nội tại hệ thống mở rộng. loại này cũng được lưu trong csdl resource nhưng có tên bắt đầu với xp_

## 3.3 Tạo thủ tục

* VÍ dụ 1: viết thủ tục nhập vào dữ liệu cho bảng HOCPHAN
## 3.4 Lời gọi thủ tục
Gọi thủ tục không cần đúng thứ tự nếu ghi rõ các tham số ra

Cách 1:
spLenDanhSachDiem 'cnpm', 'Công nghệ phần mềm', 3, 'dl01'
Cách 2:
spLenDanhSachDiem
@tenhp: 'cnpm'

## 3.5 Khai báo và sử dụng biến
### 3.5.1: Khai báo biến
```sql
DECLARE @tên_biến kiểu dữ liệu
```
- Tên biến phải bắt đầu với kí tự @

### 3.5.2: phát biểu set
- Phát biểu set dùng để gán giá trị cho các biến. Cú pháp của phát biểu SET như sau:
```sql
SET <@tên_biến> = <Giá trị> | (<PHát biểu SELECT>)
```
- Bạn có thể sử dụng nhiều phát biểu set trên cùng một dòng bằng cách sử dụng dấu chấm phẩy để phân cách
```sql
SET @Tong = 0; SET @dem = 0

set @max=(select max(diemlan1) from diemthi where mahocphan = 'sql')
```

- Chú ý: Nếu sử dụng phát biểu set với phát biểu select, phải bảo đảm phát biểu select này trả về giá trị đơn. nếu không sẽ có lỗi

- một trong những điểm mạnh của phát biểu select khi sử dụng để gán giá trị cho biến là cùng một lúc có thể lấy giá trị từ csdl và gán vào nhiều biến
```sql
select @max = 10, @min= 0, @tong=0
select @max = max(diemlan1), @min=min(diemlan1) from diemthi where mahocphan='sql'
```

## 3.6 giá trị trả về của tham số trong thủ tục
- xét câu lệnh sau đây:
```sql
create procedure sp_conghaiso(@a INT, @b INT, @c INT)
AS select @c = @a + @b
```
- thực thi một tập các câu lệnh như sau:
```sql
declare @tong int
select @tong = 0
execute sp_conghaiso 100, 200, @tong
select @tong
```
Kết quả tong =0

- nếu muốn giữ lại giá trị của đối số sau khi kết thúc thủ tục, bạn phải khai báo tham số theo cú pháp như sau:
```sql
@tên_tham_số kiểu_dữ_liệu OUTPUT ( hoặc OUT)
```
- và trong lời gọi thủ tục, sau đối số được truyền cho thủ tục, bạn cũng phải chỉ định thêm từ khoá OUTPUT(hoặc OUT)


- định nghĩa lại thủ tục ở ví dụ trên như sau:
```sql
create procedure sp_conghaiso(
    @a INT, @b INT, @c INT OUTPUT
) 
as select @c = @a + @b
```
## 3.7 cấu trúc điều khiển
### 3.7.1 cấu trúc if...else
cấu trúc như sau: 
```sql
IF <điều kiện> 
    <phát biểu 1> 
ELSE 
    <phát biểu 2>
```
- trong cấu trúc if...else, nếu có từ 2 lệnh trở lên thì phải đặt giữa 2 từ khoá begin và end.
### 3.7.2: cấu trúc while
- cấu trúc điều khiển ưhwie cho phép chúng ta lặp lại thực thi tập lệnh cho đến khi biểu thức điều kiện là false. cấu trúc như sau:
```sql
WHILE <điều kiện> 
    <phát biểu 1> 

```

ví dụ:
```sql

```
### 3.7.3: phát biểu continue
- phát biểu continue cho phép bỏ qua các lệnh còn lại trong vòng lặp hiện tại và quay lại đầu vòng lặp để kiểm tra điều kiện. cấu trúc như sau:
- phát biểu này thường được sử dụng trong vòng lặp while
- ví dụ 
### 3.7.4: phát biểu break
- phát biểu này cho phép thoát khởi vòng lặp hoặc rẽ nhánh
### 3.7.5 phát biểu return
- khi cần trả về một giá trị nào đó. chúng ta sử dụng phát biểu return. nếu gặp return, quá trình xử lý sẽ kết thúc
- trong thủ tục đôi khi chúng ta sử dụng phát biểu return để stored procedure trả về giá trị tương tự như hàm. cú pháp phát biểu này như sau:
```sql
RETURN <giá trị>
```
- ví dụ: viết thủ tục đưa vào một msv, thủ tục xuất ra số môn thi lần một không đạt của sinh viên đó
```sql
create procedure thilai(@masv nvarchar(10))
as
begin
declare @count int
select @count =(select count(*) from diemthi where masv=@masv and diemlan1<5)
return @count
end
```
- sau khi đã tạo thủ tục thilai, bạn thực thi một tập các câu lệnh như sau:
```sql
declare @dem int
exec @dem=thilai 'dl01-002'
print N'Số môn thi lại' + str(@dem)
```
### 3.7.6: cấu trúc try...catch
- một nhóm các lệnh được đặt trong khối try, nếu có một lỗi xuất hiện bên trong khối try thì điều khiển được gửi đến một nhóm lệnh khác được đặt trong một khối catch
- cấu trúc như sau:
```sql
BEGIN TRY
    <phát biểu 1>
END TRY
BEGIN CATCH
    <phát biểu 2>
END CATCH
```
- một số thông tin về lỗi
    - error_number(): trả về mã số lỗi
    - error_severity(): trả về mức độ của lỗi
- các hàm này trả về null nếu nó được gọi bên ngoài của khối catch
- ví dụ: điều khiển lỗi trong thủ tục chia 2 số
```sql
create procedure phepchia(@sobichia float, @sochia float)
as 
begin
declare @thuong float
begin try
    set @thong = @sobichia / @sochia
    print @thuong
end try
begin catch
    select
        error_number() as Error_number,
        error_severity() as ErrorSeverity,
        error_state() as ErrorState,
        error_procedure() as ErrorProcedure,
        error_line() as ErrorLine,
        error_message() as ErrorMessage;
    print N'Số chia bằng 0'
end catch
end
```
### 3.7.7: cấu trúc case
- cấu trú này có cú pháp như sau:
```sql
case biểu thức
    when biểu_thức_kiểm_tra then kết_quả
    [...]
    [else kết_quả_của_else]
end
```
- ví dụ(<strong>BTVN</strong>): nhập vào một masv, mahocphan
    - nếu điểm lần 1>=9 thì hiển thị ra câu thông báo:bạn "họ tên sinh viên" học học phần "tên học phần" xuất sắc
    - nếu 9>điểm>=8 hiển thị giỏi
    - nếu 8>điểm>=7 hiển thị khá
    - nếu 7>điểm>=5 hiển thị trung bình
    - nếu < 5 hiển thị yếu

## 3.8: tham số với giá trị mặc định
- giá trị mặc định sẽ được gán cho tham số trong trường hợp không truyền đối số cho tham số khi có lời gọi thủ tục
- ví dụ:
```sql
create proc sp_testDefault(@tenlop nvarchar(30)=null, @noisinh nvarchar(100)='Huế')
as 
    begin
        if @tenlop is null
            select hodem, ten
            from sinhvien 
            inner join lop on sinhvien.malop = lop.malop
```
- thực hiện các lời gọi thủ tục với các mục đích khác nhau như sau:
    - cho biết họ tên của các sinh viên tại huế: sp_testdefault
    - cho biết họ tên của các sinh viên lớp dữ liệu 2 sinh tại huế: sp_testdefault @tenlop=n'Dữ liệu 2'
    - cho biết họ tên của các sinh viên sinh tại quảng nam : sp_testdefault @noisinh=n'Quảng Nam'
    - cho biết họ tên của các sinh viên lớp đồ hoạ 3 sinh tại đà nẵng: sp_testdefault @tenlop=n'Đồ hoạ 3', @noisinh=n'Đà Nẵng'
## 3.9: sửa đổi thủ tục
```sql
alter procedure....
```

## 3.10: xóa thủ tục
```sql
drop procedure <tên thủ tục>
```
## 3.11: kiểu d ữ liệu cursor(con trỏ)
### 3.11.1: giới thiệu cursor
- kiểu dữ liệu cursor được sử dụng trong trường hợp tính toán trên tập bản ghi, nên nó cho phép định vị được bản ghi cần xử lý trong tập dữ liệu. cũng có thể lấy ra một hay nhiều bản ghi từ vị trí hiện hành trong tập dữ  liệu kết quả
- cursor cũng cho phép cập nhật bản ghi trong tập dữ liệu tại vị trí cursor hiện hành. bên cạnh đó, bạn cũng có thể cho phép che dấu/hiển thị bản ghi trong tập dữ liệu
- sql server cung cấp 2 phương thức để gọi cursor là t-sql và hàm database api
-chú ý : 1 ứng dụng không nên lẫn lộn giữa 2 phương thức gọi cursor

### 3.11.2: quá trình xử lý của cursor
#### 3.11.2.1: khai báo cursor
- khai báo cursor sử dụng t-sql có cú pháp như sau:
```sql
declare tên_cursor
[insensitive | sensitive] [scroll | forward_only]
cursor for câu_lệnh_select
[for [read only | update] [of <tên_cột>]] 
```
- insensitive: cho phép định nghĩa 
#### 3.11.2.2
#### 3.11.2.3: lấy dữ liệu
#### 3.11.2.4: duyệt bản ghi
- để duyệt qua từng bản ghi trong cursor, sử dụng hàm @@fetch_status và phát biểu while. hàm @@fetch_status trả về giá trị 0 ứng với bản ghi lấy ra hợp lệ
- phát biểu 
#### 3.11.2.5: đóng và giải phóng bộ nhớ cho biến cursor
- sau khi kết thúc làm việc với cursor, bạn cần khai báo để đóng cursor và giải phóng bộ nhớ đã cấp cho nó với cú pháp như sau:
```sql
CLOSE <tên_cursor>
DEALLOCATE <tên_cursor>
```
