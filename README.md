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
* Mở terminal
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
