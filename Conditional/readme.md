# ABOUT ASM'S CONDITIONAL

	Phần này sẽ nói về conditional và `call`, `ret` trong ASM. Thông số và ví dụ có thể sẽ lấy từ cả x86 lẫn x86-64. 

---

## A. CONDITIONAL VỚI ĐIỀU KIỆN LÀ CMP TRONG ASM 

---

### Tổng quan 

- Một số lưu ý trước khi bắt đầu 

1. `JZ`=`JE`

2. Dùng `JG`,`JL` khi làm việc với số có dấu (Signed)

3. Dùng `JA`,`JB` khi làm việc với số không dấu (Unsigned) 

---

### 1. LỆNH CMP 

- CMP Là lệnh dùng để so sánh 2 giá trị toán hạng mà không làm thay đổi chúng 

- Cú pháp : 
```assembly
cmp destination, source
```

- Nó thực ra là thực hiện phép trừ `destination - source` và dựng flag (thiết lập cờ) để quyết định điều kiện nhảy (có vẻ kha khá giống với () trong if(){} else)

- Các flag quan trọng cần lưu ý :

|Flag| |Tác dụng|
|----|-|--------|
|`ZF`|Zero Flag|Bật nếu 2 giá trị bằng nhau|
|`CF`|Carry Flag|Bật nếu có số dư khi trừ số không dấu|
|`SF`|Sign Flag|Bật nếu kết quả là số âm|

---

### 2. Lệnh nhảy không điều kiện (JMP)

- Dùng để chuyển ngay đến một vị trí khác mà không cần kiểm tra điều kiện 

- Ví dụ :
```assembly
mov rax, [variable_1]
jmp skip
mov rax, [variable_2]	;Dòng này bị skip

skip:
```

---

### 3. Lệnh nhảy có điều kiện (JE, JNE, JG, JL,...)

- Các lệnh nhảy có điều kiện cho phép chương trình quyết định nhảy dựa trên kết quả của `CMP`

- Ví dụ 
```assembly
cmp eax, [variable]	;Kiểm tra điều kiện
JE equal_label	

equal_label:
; Ý nghĩa đoạn code này là 'nếu eax == [var] thì nhảy đến label chỉ định'
```

- Một số lệnh nhảy quan trọng :

|Lệnh|Ý nghĩa|Ktra cờ|
|----|-------|-------|
|`JE`/`JZ`|Jump if equal/Jump if Zero|ZF|
|`JNE`/`JNZ`|Jump if not equal/ not Zero|ZF|
|`JG`/`JNLE`|Jump if greater/not less or equal|SF,ZF|
|`JGE`/`JNL`|Jump if greater or equal/not less|SF|
|`JL`/`JNGE`|Jump if less/not greater or equal|SF|
|`JLE`/`JNG`|Jump if less or equal/not greater|SF,ZF|

---

## B. CONDITIONAL VỚI LỆNH NHẢY PHỤ THUỘC VÀO FLAG 

- Những lệnh này kiểm tra trực tiếp các cờ (Flags) thay vì sử dụng CMP như câu điều kiện để so sánh

- Danh sách các lệnh quan trọng :

|Lệnh		|Ý nghĩa					|Flag bị kiểm tra	|
|-----------|---------------------------|-------------------|
|`JXCZ`		|Jump if CX is Zero			|Không kiểm tra		|
|`JC`		|Jump if Carry (Nhảy nếu có carry - tràn số 0 dấu)|CF=1|
|`JNC`		|Jump if no Carry (Nhảy nếu 0 có carry)|CF=0|
|`JO`		|Jump if Overflow (Nhảy nếu tràn số ko dấu)|OF=0|
|`JNO`		|Jump if not Ovf (Nhảy nếu không có tràn số)|OF=0|
|`JP`/`JPE` ||PF=1|
|`JNP`/`JPO`||PF=0|
|`JS`		|Jump if sign (Nhảy nếu số âm)|SF=1|
|`JNS`		|Jump if No Sign (Nhảy nếu số dương)|SF=0|

- 