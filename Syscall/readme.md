# SYSCALL TRONG ASSEMBLY ( CHỈ ĐỐI VỚI x86_64 và 32-bits )

## Tổng Quan
Tài liệu này cung cấp danh sách các lệnh Assembly phổ biến trong kiến trúc `x86-64` trên Linux, cùng với mục đích, cách dùng và ví dụ thực tế.

---

## A. CÁC LỆNH CƠ BẢN TRONG ASM

### 1. Lệnh Di Chuyển Dữ Liệu (Data Movement)

| Lệnh | Mục Đích | Cách Dùng | Ví Dụ |
|------|---------|----------|--------|
| `mov` | Sao chép giá trị giữa các thanh ghi hoặc bộ nhớ | `mov rax, rbx` | `mov rdi, 5` |
| `movzx` | Chuyển số không dấu và mở rộng giá trị | `movzx rax, bx` | `movzx eax, byte [var]` |
| `movsx` | Chuyển số có dấu và mở rộng giá trị | `movsx rax, bx` | `movsx eax, word [var]` |
| `lea` | Lấy địa chỉ bộ nhớ và lưu vào thanh ghi | `lea rax, [rbx+4]` | `lea rdi, [msg]` |

---

### 2. Lệnh Số Học (Arithmetic)

| Lệnh | Mục Đích | Cách Dùng | Ví Dụ |
|------|---------|----------|--------|
| `add` | Cộng hai số | `add rax, rbx` | `add eax, 10` |
| `sub` | Trừ hai số | `sub rax, rbx` | `sub eax, 5` |
| `mul` | Nhân hai số không dấu | `mul rbx` | `mul rdx` |
| `imul` | Nhân hai số có dấu | `imul rax, rbx` | `imul eax, 5` |
| `div` | Chia số không dấu | `div rbx` | `mov rax, 100` + `div rcx` |
| `idiv` | Chia số có dấu | `idiv rbx` | `idiv eax` |

---

### 3. Lệnh Logic (Bitwise Operations)

| Lệnh | Mục Đích | Cách Dùng | Ví Dụ |
|------|---------|----------|--------|
| `and` | Thực hiện phép AND bitwise | `and rax, rbx` | `and eax, 0xFF` |
| `or` | Thực hiện phép OR bitwise | `or rax, rbx` | `or eax, 1` |
| `xor` | Thực hiện phép XOR bitwise (cũng dùng để xóa thanh ghi) | `xor rax, rax` | `xor eax, ebx` |
| `not` | Đảo ngược bit | `not rax` | `not eax` |

---

### 4. Lệnh Dịch Bit (Bit Shift)

| Lệnh | Mục Đích | Cách Dùng | Ví Dụ |
|------|---------|----------|--------|
| `shl` | Dịch trái | `shl rax, 1` | `shl eax, 2` |
| `shr` | Dịch phải | `shr rax, 1` | `shr eax, 3` |
| `sal` | Dịch trái có dấu (giống `shl`) | `sal rax, 2` | `sal eax, 1` |
| `sar` | Dịch phải có dấu | `sar rax, 2` | `sar eax, 3` |

---

### 5. Lệnh Nhảy và Điều Kiện (Jump & Control Flow)

| Lệnh | Mục Đích | Cách Dùng | Ví Dụ |
|------|---------|----------|--------|
| `jmp` | Nhảy không điều kiện | `jmp label` | `jmp end_loop` |
| `je` | Nhảy nếu bằng (`ZF = 1`) | `je label` | `cmp eax, 10` + `je exit` |
| `jne` | Nhảy nếu không bằng (`ZF = 0`) | `jne label` | `cmp eax, 5` + `jne loop` |
| `jg` | Nhảy nếu lớn hơn | `jg label` | `cmp eax, ebx` + `jg greater` |
| `jl` | Nhảy nếu nhỏ hơn | `jl label` | `cmp eax, 0` + `jl negative` |

---

### 6. Lệnh Quản Lý Stack (Stack Operations)

| Lệnh | Mục Đích | Cách Dùng | Ví Dụ |
|------|---------|----------|--------|
| `push` | Đẩy giá trị vào stack | `push rax` | `push 10` |
| `pop` | Lấy giá trị từ stack | `pop rax` | `pop rbx` |
| `call` | Gọi hàm | `call function_name` | `call printf` |
| `ret` | Trả về từ hàm | `ret` | `ret` |

---

### 7. Lệnh Hệ Thống (Syscall & Interrupts)

| Lệnh | Mục Đích | Cách Dùng | Ví Dụ |
|------|---------|----------|--------|
| `syscall` | Gọi hệ thống Linux | `syscall` | `mov rax, 60` + `syscall` (thoát chương trình) |
| `int` | Ngắt phần mềm | `int 0x80` | `mov eax, 1` + `int 0x80` (thoát chương trình) |

---

### 8. Lệnh Đặc Biệt (Special Instructions)

| Lệnh | Mục Đích | Cách Dùng | Ví Dụ |
|------|---------|----------|--------|
| `nop` | Không làm gì cả (No Operation) | `nop` | `nop` |
| `cpuid` | Lấy thông tin CPU | `cpuid` | `mov eax, 1` + `cpuid` |
| `hlt` | Dừng CPU | `hlt` | `hlt` |

---

## B. CÁC SYSCALL CHÍNH TRONG ASM ( KHÔNG BAO GỒM CALL libc )

