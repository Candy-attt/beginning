Thanh ghi là vùng lưu trữ nhỏ, tốc độ cao nằm bên trong CPU.

Các thanh ghi quan trọng:
Thanh ghi	Chức năng
RAX	Thanh ghi đa dụng, thường chứa kết quả
RBX	Thanh ghi đa dụng
RCX	Thanh ghi đa dụng, thường chứa đối số
RDX	Thanh ghi đa dụng
RSI	Source Index
RDI	Destination Index
RSP	Stack Pointer
RBP	Base Pointer
RIP	Instruction Pointer
RFLAGS	Các cờ trạng thái CPU

Trong x86, các thanh ghi tương ứng thường có tiền tố E, ví dụ EAX, ESP, EIP.

Bạn cần đặc biệt hiểu:
- RAX: Giá trị dữ liệu và kết quả phép tính.
- RSP: Vị trí hiện tại của đỉnh Stack.
- RBP: Thường dùng làm mốc tham chiếu trong Stack Frame.
- RIP: Địa chỉ lệnh CPU đang thực thi.
Điểm quan trọng cần nhớ khi học RE: CMP thiết lập các CPU Flags, còn JE, JNE, JG, JL sử dụng những Flags đó để quyết định luồng thực thi.
<img width="1532" height="602" alt="image" src="https://github.com/user-attachments/assets/9fc71ff5-00a1-42e7-a7e2-ddd819e1f418" />
Quy ước này quy định các tham số nguyên hoặc con trỏ đầu tiên được truyền qua những thanh ghi sau:
Tham số	Thanh ghi
a (tham số 1)	RDI
b (tham số 2)	RSI
Tham số 3	RDX
Tham số 4	RCX
Tham số 5	R8
Tham số 6	R9
Các thanh ghi đa năng chính (General-Purpose Registers)
RAX / EAX (Accumulator): Thanh ghi tích lũy; thường chứa giá trị trả về của hàm.

RBX / EBX (Base): Thanh ghi cơ sở; thường dùng làm con trỏ trỏ tới dữ liệu trong bộ nhớ.

RCX / ECX (Counter): Thanh ghi đếm; sử dụng trong các vòng lặp (loop) hoặc thao tác chuỗi.

RDX / EDX (Data): Thanh ghi dữ liệu; hỗ trợ thanh ghi RAX trong các phép nhân/chia lớn hoặc lưu trữ dữ liệu I/O.

RSI / ESI (Source Index): Con trỏ nguồn cho các thao tác sao chép dữ liệu, chuỗi ký tự.

RDI / EDI (Destination Index): Con trỏ đích cho các thao tác chuỗi.

RBP / EBP (Base Pointer): Con trỏ khung stack (Frame Pointer); dùng để đánh dấu mốc cơ sở của khung hàm hiện tại.

RSP / ESP (Stack Pointer): Con trỏ ngăn xếp; luôn trỏ tới đỉnh hiện tại của Stack.

R8 đến R15: Các thanh ghi bổ sung mới xuất hiện trên kiến trúc x64, giúp tăng hiệu suất mà không cần lạm dụng stack.
RSP là con trỏ trỏ đến phần tử ở đỉnh stack, Stack phát triển từ trên xuống dưới (từ địa chỉ cao đến địa chỉ thấp).
Heap nằm ở vùng địa chỉ thấp hơn Stack và phát triển theo chiều hướng lên (Low to High).
3. Xem code mà chưa chạy
Lệnh	Tác dụng
info functions	liệt kê các hàm
disassemble main	xem assembly của main
x/20i main	xem 20 lệnh từ main
info files	xem các vùng của file
4. Đặt điểm dừng (breakpoint) và chạy
Lệnh	Tác dụng
break main	dừng ở đầu main
break *main+20	dừng đúng tại một lệnh (offset từ main)
break *0x...	dừng tại địa chỉ cụ thể
info breakpoints	xem danh sách breakpoint
delete 1	xóa breakpoint số 1
run (hoặc r)	chạy chương trình
run < input.txt	chạy với dữ liệu vào từ file
continue (hoặc c)	chạy tiếp tới breakpoint sau

Lưu ý về file PIE (như asciiftw): địa chỉ thay đổi mỗi lần chạy. Dùng dạng break main hoặc break *main+N (theo offset) sẽ luôn đúng, còn địa chỉ tuyệt đối chỉ dùng được sau khi đã run.

5. Chạy từng bước
Lệnh	Tác dụng
stepi (hoặc si)	chạy một lệnh assembly, đi vào trong hàm được gọi
nexti (hoặc ni)	chạy một lệnh, bước qua lời gọi hàm
finish	chạy tới khi hàm hiện tại kết thúc
until *địa_chỉ	chạy tới một địa chỉ

Tương tự F7/F8 trong x32dbg bạn đã dùng.

6. Xem thanh ghi và bộ nhớ
(gdb) info registers           # tất cả thanh ghi
(gdb) print $eax               # một thanh ghi
(gdb) print/x $rax             # dạng hex
(gdb) print/d $eax             # dạng thập phân
(gdb) x/s 0x...                # xem chuỗi tại địa chỉ
(gdb) x/20xb $rsp              # 20 byte hex tại stack
(gdb) x/4gx $rsp               # 4 giá trị 8 byte (hex)
(gdb) x/10i $pc                # 10 lệnh từ vị trí hiện tại

Cú pháp x/NFU địa_chỉ: N là số lượng, F là định dạng (x hex, d thập phân, s chuỗi, i lệnh, c ký tự), U là kích thước (b byte, h 2 byte, w 4 byte, g 8 byte).

7. Sửa giá trị lúc chạy
(gdb) set $eax = 0
(gdb) set *(int*)0x... = 5

Chính là thao tác bạn đã làm với EAX ở bài WinAntiDbg trong x32dbg, chỉ khác công cụ.
<img width="657" height="172" alt="image" src="https://github.com/user-attachments/assets/17f4f479-7eca-4450-8e80-19bfff4d4056" />
