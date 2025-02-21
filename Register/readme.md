# Thanh ghi trong ASM ( Hay còn gọi là register )

## A. Các loại thanh ghi chính bao gồm :

1. General Purpose Registers ( GPR ) 

- Là các thanh ghi đa dụng.

- Bao gồm : < Thông số lấy từ system 32-bit >

|Register|Kích Thước|Chức năng chính|
|:------:|:--------:|:-------------:|
|eax     |32-bit	|Accumulator - dùng để tính toán, kết quả return về syscall|
|ebx     |32-bit    |Base Register - dùng làm biến tạm hoặc truyền tham số|
|ecx     |32-bit    |Counter Register - dùng trong **LOOP** hoặc truyền tham số|
|edx     |32-bit    |Data Register - Thường dùng để chứa số dư khi chia / truyền tham số|

2. Special Purpose Registers ( SPR )