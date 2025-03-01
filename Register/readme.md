# Thanh ghi trong ASM (Register)

## A. Các loại thanh ghi chính bao gồm :

---

1. General Purpose Registers (GPR) 

- Là các thanh ghi đa dụng.

- Bao gồm : < Thông số lấy từ system 32-bit >

|Register|Kích Thước|Chức năng chính|
|:------:|:--------:|:-------------:|
|`eax`   |32-bit	  |Accumulator - dùng để tính toán, kết quả return về syscall|
|`ebx`   |32-bit    |Base Register - dùng làm biến tạm hoặc truyền tham số|
|`ecx`   |32-bit    |Counter Register - dùng trong **LOOP** hoặc truyền tham số|
|`edx`   |32-bit    |Data Register - Thường dùng để chứa số dư khi chia / truyền tham số|

---

2. Special Purpose Registers (SPR)

- Là các thanh ghi điều khiển CPU

|Register|Size|Chức năng chính|
|:------:|----|---------------|
|`esp`   |32-bit|Stack pointer, dùng để làm pointer khi call stack frame|
|`ebp`   |32-bit|Base pointer|
|`eip`   |32-bit|Instruction pointer|

---

3. Segment register (SR)

- Chỉ dùng cho quản lí bộ nhớ

|Register|Chức năng|
|--------|---------|
|`cs`    |Code segment, quản lí mã lệnh|
|`ds`    |Data segment, quản lí dữ liệu|
|`ss`    |Stack segment, quản lí stack|

---

## Vậy là hết kiến thức về phần Register. Sau đây có một vài lưu ý sau :
⚠️ Có 2 loại call phổ biến là **syscall** và **int 0x80**. **int 0x80** sẽ dùng cho hệ thống x86, khi call nó sẽ kích hoạt hệ thống chuyển quyền vào kernel, còn **syscall** trên x86-64 sẽ thay đổi cách gọi (ở phần sau sẽ nói rõ hơn). Còn nữa, chúng ta có thể dùng syscall của x86 lên x86_64 nhưng không thể làm ngược lại :v 

