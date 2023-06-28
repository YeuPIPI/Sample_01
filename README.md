# Sample_01

# 1.Thông tin chung


![image](https://github.com/YeuPIPI/Sample_01/assets/72372550/b7fb13f9-cb76-40a3-85a4-13775f4ca9d8)

# 2.MITRE ATT&CK™ Techniques Detection
ATT&CK ID   -  	Name
T1059.003	 - Windows Command Shell
T1547.001	- Registry Run Keys / Startup Folder
T1055	- Process Injection
T1571	- Non-Standard Port
T1114	- Email Collection
T1012 - 	Query Registry
T1112 - 	Modify Registry

# 3.Phân tích chi tiết

Phân tích theo luồng thực thi của mã độc . Chia thành phân tích 3 stage chính 

## 3.1 Stage 1

![image](https://github.com/YeuPIPI/Sample_01/assets/72372550/cd38ebab-f7fb-4b4e-87bf-143f53d5f090)

Ban đầu nó lấy handle của console và ẩn nó đi . Tiếp theo nó get cái foder mà nó chỉ định create 1 directory là Explorer . Sau khi set attribute thì nó sẽ đi vào hàm Rename_and_CreateKey . 

![image](https://github.com/YeuPIPI/Sample_01/assets/72372550/e8b79e2c-5639-4fdf-8523-e952a537775b)


Ở hàm này nó sẽ copy file ban đầu là Explorer.exe đến foder mà nó tạo trước và rename thành Unikey.exe để ngụy trang . Sau đó nó sẽ create 1 key và set value key là Unikey NT nhằm mục đích persistant chương trình độc hại này

## 3.2 Stage 2

![image](https://github.com/YeuPIPI/Sample_01/assets/72372550/e7e9c5ac-70df-404c-b0a8-345f283da866)

![image](https://github.com/YeuPIPI/Sample_01/assets/72372550/29ef08ab-3364-418e-bc96-d6598fe67821)


Nó sẽ tạo 1 file tên là Tranfer.exe  và gọi tới hàm Get_info_com . Ở hàm này , đầu tiên nó sẽ tạo 1 tên txt là systeminfo.txt . Tiếp theo nó sẽ mở 1 key nếu mở thành công nó sẽ get value ở trường ProcessorNameString và ghi vào tệp systeminfo.txt . Nó sẽ đi tiếp vào hàm Get_info_software 

![image](https://github.com/YeuPIPI/Sample_01/assets/72372550/7c52a1f8-c954-45a0-9593-9fdfde5d5e0d)

ở hàm này nó , Đầu tiên nó mở 1 key là "SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Uninstall" . nếu mở thành công nó sẽ lấy các thông tin về phần mềm nhưu tên , phiên bản , InstallLocation và ghi vào tệp systeminfo.txt

## 3.3 Stage 3

![image](https://github.com/YeuPIPI/Sample_01/assets/72372550/779409f9-9bcf-4070-836e-9adeb20984c9)

Đầu tiên nó khởi tạo 1 thread và gọi tới địa chỉ nó muốn thực thi . Đi tới địa chỉ đấy 

![image](https://github.com/YeuPIPI/Sample_01/assets/72372550/997bf21f-175c-4ea7-93a4-72aa0050e5f2)

Nó sẽ check hàm keylog tiếp theo là 1 vòng lặp vô tân về việc xử lý và chuyển tiếp &Msg . ĐI vào hàm Keylog . 

![image](https://github.com/YeuPIPI/Sample_01/assets/72372550/e7f1fbe1-ed09-4a20-9dd2-fd800cbb397b)

Nó sẽ tạo 1 file dll là KeyLog.dll . Sau khi writefile xong thì nó sẽ call tới 2 hàm có trong dll đó là FillKeyboard và SetGlobalHookHandle . Đi vào dll xem 2 hàm đó làm gì 
![image](https://github.com/YeuPIPI/Sample_01/assets/72372550/e60c75d9-058d-4647-868c-4ef020f46ac3)

ở hàm FillKeyboard chúng ta có thể thấy nó đầu tiên truy cập tới foder đó tạo 1 file tên là log.txt . Đầu tiên nó sẽ check ForegroundWindow . Nó sẽ ghi vào file log time ngày thao tác . Tên đường dẫn tệp thao tác , ứng dụng thao tác và ký tự thao tác với ứng dụng đó 

Hàm SetGlobalHookHandle thì khá đơn giản nó sử dụng để lưu trữ handle của hook thôi

Quay về luồng thực thi chính chúng ta thấy nó tạo 1 vòng lặp vô tận . Nó sẽ gọi hàm Get_screen_win hàm này đầu tiên sẽ khởi 1 file tên là screen.jpeg và thực thi 1 số api nhằm mục đích là hàm chụp ảnh màn hình . Tiếp theo nó sẽ gọi hàm system là hàm thực thi command của hệ thống từ cmd . Nó sẽ thực thi command excute file Tranfer.exe và nó sẽ thực hiện lặp đi lặp lại sau 10 phút mỗi lần . 
Vì file tranfer được complier bằng .NET nên chúng ta ném nó vào dnspy để phân tích cho dễ

![image](https://github.com/YeuPIPI/Sample_01/assets/72372550/9295799b-a68d-442e-b4b3-3938d5fd9c42)

File này nó sẽ khởi tạo object là SmtpClient . Nó sẽ setup crentidal là "anhthc95@gmail.com", "123456789@A" và nó sẽ khởi tạo 1 đối tượng mailmessage và setup time . Nó sẽ send 3 file log đã lưu vào là screen.jpg , log.txt , systeminfo.txt 
















