# Viết và Hook syscalls trong Linux kernel

### Thành viên
Mai Nguyễn Anh Vũ	1612904

Nguyễn Khắc Đức	1612845

### Lý do thực hiện
- Đồ án thực hành môn Hệ điều hành.
### Kết quả
- Viết và chạy thành công 2 syscall theo yêu cầu của đề bài: nhập vào tên tiến trình, trả về PID (nếu có); và ngược lại, nhập vào PID, trả về tên tiến trình (nếu có).
- Hook thành công vào 2 syscall của kernel có sẵn: **open()** ghi vào dmesg tên tiến trình nào mở file và tên file mở; **write()** ghi vào dmesg tên tiến trình, tên file bị ghi và số byte được ghi. 
### Hệ điều hành và Ngôn ngữ được dùng
- Module tạo trên hệ điều hành **Ubuntu 16.04 LTS**, phiên bản **Linux 4.4.0**.
- File syscall, file module và test module được viết bằng C với thư viện hỗ trợ (kernel) linux.
### Cách thực hiện
#### Viết syscall và nạp vào kernel
- Mở terminal
- Ta tải linux kernel phù hợp với kernel đang chạy trên máy. Vì chúng tôi dùng kernel 4.4.0-31-generic nên sẽ tải linux kernel 4.4.0 (theo https://packages.ubuntu.com/xenial/linux-source-4.4.0). Để biết được hệ điều hành có phiên bản kernel nào, ta gõ lệnh uname -r. 
-	Sau khi tải xong, ta sẽ giải nén vào thư mục /usr/src/ bằng lệnh  tar -xvf linux-4.4.0.orig.tar.gz -C /usr/src/linux-4.4
-	Ta chuyển đường dẫn hiện tại ở terminal vào /usr/src/linux-4.4 bằng lệnh cd /usr/src/linux-4.4. Ta tạo 1 thư mục pname bằng lệnh mkdir pname
-	Ta cd vào pname (cd pname). Rồi tạo file pname.c bằng lệnh gedit pname.c. Ta chép đoạn lệnh thuộc syscall pnametoid vào file này. 
-	Ta tạo thêm 1 file pname.h, tương tự như cách tạo file pname.c
-	Ta cần tạo 1 file Makefile để chỉ thị biên dịch, tương tự như cách tạo file pname.c. 
- Sau đó, quay trở ra thư mục cha /usr/src/linux-4.4 thay đổi file Makefile như sau:

          o Ta dùng lệnh cat -n Makefile | grep -i core-y để xem giá trị ở dòng bắt đầu core-y.
          o Ta sẽ thêm pname/ vào sau cùng dòng 882 thông qua lệnh nano +882 Makefile. Đối với từng kernel khác nhau số thứ tự dòng có thể khác nhau.
-	Bước kế, ta sẽ thêm syscall pname vừa mới viết vào syscall table. Trước tiên ta phải tìm ra đường dẫn chứa file syscall_64.tbl. Theo bài làm của nhóm, đường dẫn là /usr/src/linux-4.4/arch/x86/entry/syscalls/syscall_64.tbl. Ta tiến hành thêm bằng cách dùng lệnh nano /usr/src/linux-4.4/arch/x86/entry/syscalls/syscall_64.tbl, thêm dòng sau vào: 326     common  pname                   pnametoid. Tùy vào kernel mà sẽ thêm số đầu tiên khác các số còn lại.
-	Sau đó, ta tiến hành biên dịch kernel.

          o Lệnh make menuconfig: chọn mũi tên sang trái (→) vào ô save rồi nhấn enter. Sau đó thoát (exit).
          o Nếu máy ảo có 2 nhân thì chạy lệnh make -j2 (để tăng tốc quá trình biên dịch, ta cũng tùy vào số lượng nhân mà chọn số sau j) hoặc không thì chỉ cần chạy make.
          o	Sau đó, chạy lệnh make install
          o	Tiếp đến, chạy lện make modules_install install.
-	Cuối cùng reboot.
-	Ngoài ra, nếu trong quá trình reboot, bạn cần giữ shift để vào chỉnh kernel sẽ được chạy.
-	Ta chọn Advanced options for Ubuntu. Rồi chọn Ubuntu, With Linux 4.4.0
-	Kiểm tra xem có chạy kernel thành công hay không, ta vào terminal và gõ uname -a, kiểm tra ngày tháng năm tạo của kernel.
-	Kiểm tra syscall có chạy hay không bằng cách tạo ra file testPname.c, biên dịch rồi chạy thử.
         
         o	Tạo file testPname.c bằng lệnh gedit testPname.c trong thư mục pname. 
         o	Biên dịch bằng lệnh gcc testPname.c -o testPname
         o	Rồi chạy bằng lệnh ./testPname
-	Đối với syscall pidtopname, ta làm tương tự các bước trên. Để test ta tạo ra file testPname1.c

#### Hook syscall
Để viết module dùng cho việc hook, ta cần phải đi xác định địa chỉ của sys_call_table bằng cách gọi lệnh: cat /boot/System.map-$(uname -r) | grep sys_call_table
-	Tạo 1 file tên Hook ở thư mục gốc (root). Rồi cd vào thư mục Hook.
-	Ta sẽ tạo file captainHook.c.
-	Sau đó tạo file Makefile.
-	Rồi chạy make.
-	Nạp module vào kernel bằng lệnh insmod: insmod captainHook.ko
-	Cd ra thư mục cha:  cd ..
-	Chạy ./open Desktop/ttt.txt với file open.c 
-	Rồi tháo module khỏi kernel bằng lệnh rrmod: rrmod captainHook
-	Ta mở 1 terminal khác lên. Chạy dmesg -wH để kiểm tra.

##### Lưu ý: Mã nguồn của các file trên được đính kèm trong file tổng hợp báo cáo.
### Giấy phép
Theo GPL.
### Nguồn tham khảo
Các file đã viết bên trên được thực hiện dựa vào những ví dụ trong [bài viết](
https://uwnthesis.wordpress.com/2016/12/26/basics-of-making-a-rootkit-from-syscall-to-hook/?fbclid=IwAR1UrdNovv7LaGcJ6jEToaFF3waVrBfX04wIeckcz7z1JNTpPRcFXZMStqI).

Ngoài ra, để hoàn thành đồ án này, chúng tôi còn tham khảo các link khác.

https://ubuntuforums.org/showthread.php?t=2147227&fbclid=IwAR01FZpxRMBXUdcUjmFxAXfKqZkEUwgVXsbYkXn3UR0StSqBUbHkcOehpgM

 https://askubuntu.com/questions/270381/how-do-i-install-ncurses-header-files?fbclid=IwAR3T6EnmbouRK9Ns9rVhlBDR7WDIDl3gqO__fTMJ1n7e4WZT0tjcY9FlxM0

https://medium.com/anubhav-shrimal/adding-a-hello-world-system-call-to-linux-kernel-dad32875872?fbclid=IwAR3oFS4Q0eRfKEpEkShuZeM_NPwJl6M7A83-M9L6gk4DSyhPNGfwAnTg0D4

https://packages.ubuntu.com/xenial/linux-source-4.4.0

https://ruinedsec.wordpress.com/2013/04/04/modifying-system-calls-dispatching-linux/?fbclid=IwAR2B6xsDJi94vKls8x-aBIk4vlyfsGW1mxMckSkM0l3lDPp56hYOwd4WRF4

http://www.linfo.org/system_call_number.html?fbclid=IwAR0EMogZT1gRrjFLyMrzOy6dhEozOgRaMhdIYhpwmLzt4V1UiEsaWls9fZY

https://www.wikitechy.com/tutorials/linux/how-to-get-filename-from-file-descriptor-linux-in-c?fbclid=IwAR2ymZDpN-dSyzSPwU1E_m8KsZc7mp9fyuL773Y__ph7LlFjKeiZtc6sZbE

https://stackoverflow.com/questions/8250078/how-can-i-get-a-filename-from-a-file-descriptor-inside-a-kernel-module

https://stackoverflow.com/questions/17504859/get-file-name-path-from-a-file-descriptor-from-a-linux-kernel-module

http://man7.org/linux/man-pages/man2/write.2.html

http://man7.org/linux/man-pages/man2/open.2.html

https://www.ibm.com/developerworks/community/blogs/58e72888-6340-46ac-b488-d31aa4058e9c/entry/understanding_linux_open_system_call?lang=en&fbclid=IwAR2ymZDpN-dSyzSPwU1E_m8KsZc7mp9fyuL773Y__ph7LlFjKeiZtc6sZbE

https://stackoverflow.com/questions/2103315/linux-kernel-system-call-hooking-example

https://stackoverflow.com/questions/1941464/how-to-get-a-file-pointer-from-a-file-descriptor
