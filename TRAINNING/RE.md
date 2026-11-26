1.Khái niệm về Reverse Engineering
  - Là quá trình phân tích 1 sản phẩm đã hoàn chỉnh để hiểu cách nó hoạt động mà không có sẵn mã nguồn. Đối tượng phân tích chính thường là file thực thi.
  Quá trình tạo ra chương trình:
    Mã nguồn (C/C++) -> Trình biên dịch -> Mã máy (các byte lệnh) -> CPU thực thi
  - RE đi theo hướng ngược lại: Bắt đầu từ mã máy, dùng disassembler để đọc ra mã assembly, dùng decompiler để dựng lại mã nguồn gần giống.
2.Static Analysis và Dynamic Analysis
  2.1.Static Analysis (Đọc chương trình mà không chạy chương trình)
    - Phân tích file nhị phân bằng cách đọc cấu trúc,chuỗi assembly và mã decompile mà không thực thi chương trình.
    *Tools và kỹ thuật
    file: xác định loại file.

    <img width="707" height="98" alt="image" src="https://github.com/user-attachments/assets/5aa76652-0b6e-48ab-ac96-bf4504ce883a" />
    
    -> file asciiftw: ELF64-bit ( dành cho thanh ghi x86-64 ),PIE, not stripped (còn giữ tên hàm )
    strings: liệt kê các chuỗi đọc được trong file, thường lộ thông báo, đường dẫn, đôi khi cả flag.
    
    <img width="422" height="490" alt="image" src="https://github.com/user-attachments/assets/ec985d05-9f42-4601-af03-e157fe643b5d" />
    
    objdump -d -M intel: xem assembly, các section, bảng ký hiệu.
    
    <img width="725" height="488" alt="image" src="https://github.com/user-attachments/assets/00e45d0b-41da-4213-bb77-ffe83fd190cb" />
    
      Lệnh thường dùng khi cần đọc main:
        `objdump -d -M intel ten_file | grep -A40 "<main>:"`: đọc 40 dòng sau nhãn main.
    
    <img width="630" height="477" alt="image" src="https://github.com/user-attachments/assets/05fae813-817e-4bc8-82bf-fc08819c5ba4" />

    Ghidra hoặc IDA: disassembler kèm decompiler,cho phép đổi tên biến, xem tham chiếu chéo, tìm chuỗi bằng Defined Strings.
    Cách thường dùng:
      - Import file
        
        <img width="795" height="593" alt="image" src="https://github.com/user-attachments/assets/47f7aafd-ba96-4754-9976-5384502d419b" />
        
      - Chạy auto-analysis
        
        <img width="995" height="595" alt="image" src="https://github.com/user-attachments/assets/8ece7c3e-f744-49b2-a072-f04263e664f6" />
        
      - Tìm hàm cần xem: thường là hàm main nếu file không bị strip hoặc theo chuỗi kí tự
        
        <img width="227" height="258" alt="image" src="https://github.com/user-attachments/assets/6c60e38d-3906-417c-bf60-2a8e00905649" />
        
        <img width="403" height="287" alt="image" src="https://github.com/user-attachments/assets/cdd9e049-9bc4-404b-81ec-3e8940488cc0" />
        
      - Dùng References to để đi tới code dùng chuỗi
      - Đọc cửa sổ Decompile
      - 
        <img width="1625" height="837" alt="image" src="https://github.com/user-attachments/assets/05f14e64-1c0d-4b43-9ab6-d63804b4abde" />
  2.2.Dynamic Analysis ( chạy chương trình và quan sát hành vi )
    - Phân tích bằng cách chạy chương trình, thường dưới sự kiểm soát của debugger, để quan sát hành vi thực tế: giá trị thanh ghi, nội dung bộ nhớ, các lời gọi hàm, dữ liệu vào ra.
        




