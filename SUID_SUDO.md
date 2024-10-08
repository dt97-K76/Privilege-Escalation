# Tổng quan
### Các quyền của 1 tệp tin (file):
- Read (r): 4
- Write (w): 2
- Execute (x): 1

All permissions = 777 ==> -rwxrwxrwx 

Trong đó:
- `-`: kí hiệu xác định là tệp tin (file), nếu là thư mục kí hiệu là d.
- rwx: quyền của user (chủ sở hữu).
- rwx: quyền của nhóm-group.
- rwx: quyền của người dùng khác.

Ví dụ: chmod 775 file.txt --> -rwxrwwxr-x

### Các quyền đặc biệt
- SUID (set user ID): khi set bit 4 cho trường user (chủ sở hữu), kí hiệu là s.
- SGID (set group ID): khi set bit 2 cho trường group, kí hiệu là s.
- Sticky bits: khi set bit 1 cho trường others (được phép tạo hoặc xóa bất kỳ tệp nào trong một thư mục), kí hiệu là t.

SUID permission cho phép người dùng thực thi tệp tin đó với quyền của người dùng khác (thường là root). Ví dụ: đang truy cập hệ thống đích với tư cách là người dùng không phải root và chúng tôi đã tìm thấy các tệp nhị phân được kích hoạt bit `suid`, thì các tệp/chương trình/lệnh đó có thể chạy với đặc quyền root.

Để set bit `suid` cho file ta có thể: `chomd 4777 file` hoặc `chmod u+s file`. Khi đó ở trường user thay vì `rwx` --> `rws`. (thay thế x thành s).

Để tìm các file được set bit suid ta thực hiện câu lệnh sau:

    find / -perm -u=s -type f 2>/dev/null
hoặc

    find / -perm /4000 -type f 2>/dev/null

### Sudoers file
Khái niệm: sudoers file là tệp lưu trữ người dùng và nhóm có quyền root để chạy một số hoặc tất cả các lệnh với quyền root hoặc người dùng khác.

![image](https://github.com/user-attachments/assets/685d8b15-8c02-4648-9004-48ba5bc4ecb0)

Để chỉnh sửa ta dùng lệnh `visudo`.

Ví dụ: root ALL=(ALL:ALL) ALL

- root: tên người dùng
- ALL(1): host (terminal)
- (ALL:ALL): run as (user:group)
- ALL: câu lệnh thực thi

Chạy lệnh: `sudo -l` để kiểm tra quyền sudo của người dùng hiện tại:
- Nếu `user ALL=ALL` or `user ALL=(root) ALL` thì dùng lệnh `sudo su` hoặc `sudo bash` để chuyển đặc quyền sang **root**.
- Nếu user chỉ được set chạy có tệp thực thi với quyền root như sau `user ALL=(root) NOPASSWD: /usr/bin/find` thì sử dụng `sudo find /home -exec /bin/bash \;` để nâng cao đặc quyền.


