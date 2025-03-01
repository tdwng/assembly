# SYSCALL TRONG ASSEMBLY ( CHỈ ĐỐI VỚI x86_64 và x86 32-bit )

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

### Tổng Quan
Hệ điều hành Linux cung cấp các **syscall (system calls)** để chương trình có thể giao tiếp với kernel. Syscall giúp thực hiện các tác vụ như đọc/ghi file, quản lý bộ nhớ, tạo tiến trình, v.v.

### B1. Cách Truyền Tham Số Khi Gọi Syscall

#### **1. Trên x86_64 (64-bit)**
| Thanh ghi | Chức năng |
|-----------|----------|
| `rax`     | Số syscall |
| `rdi`     | Tham số thứ 1 |
| `rsi`     | Tham số thứ 2 |
| `rdx`     | Tham số thứ 3 |
| `r10`     | Tham số thứ 4 |
| `r8`      | Tham số thứ 5 |
| `r9`      | Tham số thứ 6 |

**Ví dụ: Gọi `write()` trong Assembly (x86_64)**
```assembly
mov rax, 1      ; Syscall number của write (1)
mov rdi, 1      ; File descriptor 1 (stdout)
mov rsi, msg    ; Địa chỉ buffer chứa dữ liệu
mov rdx, len    ; Độ dài dữ liệu cần ghi
syscall
```
**Call write libc (syscall write() trong C)**
```C
#include <unistd>
int main() {
    write(1, "Hello, ditme!\n", 14);
    return 0;
}
```
#### **2. Trên x86_32 (32-bit)**

| Thanh ghi | Chức năng |
|-----------|-----------|
| `eax`     | Số syscall |
| `ebx`     | Tham số 1 |
| `ecx`     | Tham số 2 |
| `edx`     | Tham số 3 |
| `esi`     | Tham số 4 |
| `edi`     | Tham số 5 |

**Call write libc (syscall write() trong C)**
```C
#include <unistd.h>

int main() {
    syscall(4, 1, "Hello, ditme!\n", 14);
    return 0;
}
```
- Phần sau tự tìm hiểu, lười ghi quá 😥

---

### B2. DANH SÁCH CÁC SYSCALL QUAN TRỌNG :

#### ⚠️ :
1. Syscall trên x86_64 sử dụng **syscall** thay vì **int x80** như trên x86 32-bit.

2. Các tham số truyền vào thanh ghi theo quy tắc của từng kiến trúc. 

#### 1. Syscall Trên x86_64 (64-bit)
| Syscall      | Số syscall | Chức năng               | Tham số |
|-------------|------------|-------------------------|--------------------------------------------|
| `sys_write` | `1`        | Ghi dữ liệu ra file     | `rdi` = fd, `rsi` = buffer, `rdx` = length |
| `sys_exit`  | `60`       | Thoát chương trình      | `rdi` = exit code |
| `sys_read`  | `0`        | Đọc dữ liệu từ file     | `rdi` = fd, `rsi` = buffer, `rdx` = length |
| `sys_open`  | `2`        | Mở file                 | `rdi` = filename, `rsi` = flags, `rdx` = mode |
| `sys_close` | `3`        | Đóng file               | `rdi` = fd |
| `sys_fork`  | `57`       | Tạo tiến trình mới      | Không có |
| `sys_execve`| `59`       | Chạy chương trình mới   | `rdi` = path, `rsi` = argv, `rdx` = envp |
| `sys_brk`   | `12`       | Cấp phát bộ nhớ         | `rdi` = địa chỉ mới |
| `sys_mmap`  | `9`        | Map vùng nhớ            | `rdi` = addr, `rsi` = length, `rdx` = prot, `r10` = flags, `r8` = fd, `r9` = offset |

---

#### 2. Syscall Trên x86 (32-bit)
| Syscall      | Số syscall | Chức năng               | Tham số |
|-------------|------------|-------------------------|--------------------------------------------|
| `sys_write` | `4`        | Ghi dữ liệu ra file     | `ebx` = fd, `ecx` = buffer, `edx` = length |
| `sys_exit`  | `1`        | Thoát chương trình      | `ebx` = exit code |
| `sys_read`  | `3`        | Đọc dữ liệu từ file     | `ebx` = fd, `ecx` = buffer, `edx` = length |
| `sys_open`  | `5`        | Mở file                 | `ebx` = filename, `ecx` = flags, `edx` = mode |
| `sys_close` | `6`        | Đóng file               | `ebx` = fd |
| `sys_fork`  | `2`        | Tạo tiến trình mới      | Không có |
| `sys_execve`| `11`       | Chạy chương trình mới   | `ebx` = path, `ecx` = argv, `edx` = envp |
| `sys_brk`   | `45`       | Cấp phát bộ nhớ         | `ebx` = địa chỉ mới |
| `sys_mmap2` | `192`      | Map vùng nhớ            | `ebx` = addr, `ecx` = length, `edx` = prot, `esi` = flags, `edi` = fd, `ebp` = offset |

---
  
Còn tiếp...

