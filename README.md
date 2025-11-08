# Learn  MakeFile

# Chạy main.c với folder 1 file .c
```bash
gcc main.c -o main.exe 
cmd: main.exe  
MYNGW64: ./mainexe  
```

# Chạy folder có nhiêu file .c .h
Cách 1: 
```bash
gcc -c main.c  -o main.o
gcc  -c lib.c -o lib.o
gcc main.o lib.o -o main.exe
 ./mainexe  
```
trong đó:  
'-c' là tạo các file object  
'-o' là tạo ra file output  


Cách 2: Gộp các lệnh  ở cách 1 bằng việc dùng '-i'  
```bash
gcc main.c lib.c -o main.exe -I.
 ./mainexe  
```

# Makefile đầu tiên
Tạo 1 file "Makefile"  
```bash
main:
	gcc main.c lib.c -o main.exe -I.
```
Cách chạy   
```bash
make main
```

# Rule MakeFile
![alt text](image.png)\

# Biến trong makeFile
Được khởi tạo:   
```bash
NAME = VALUE
```
Được call:  
```bash
${NAME} hoặc $(NAME) 
```
# Một vài ký tự trong MAKEFILE cần nhớ wildcarrds, automatic variable  
 ## %.o  %.c  pattern đại diện cho file có phần tên bất kỳ, phần mở rộng .c hoặc .o.  
Ví dụ:
```bash
%.o: %.c
	gcc -c $< -o $@
```
Nghĩa là: “Muốn tạo file .o từ file .c có cùng tên phần đầu, chạy lệnh bên dưới”.

## Automatic variables  
```bash
$@: tên của target hiện tại (file đích)  
$<:  tên của file phụ thuộc đầu tiên (first prerequisite)  
$^ — tất cả các file phụ thuộc (all prerequisites), không trùng lặp  
```
Ví dụ minh họa:  
```bash
main.o: main.c
	gcc -c main.c -o main.o
```
Có thể viết ngắn gọn với pattern và automatic variables:  
```bash
%.o: %.c
	gcc -c $< -o $@
```
Ở đây:  
* Nếu target là main.o, thì $@ = main.o  
* File nguồn đầu tiên $< = main.c  


# PHONY
Khi target trung tên file, vd có lệnh:  
```bash
clean:
	rm -f *.exe *.o
```
Và có file 'clean' thì sẽ cho kết quả: 
```bash
make : 'clean' is up to date.
```
ta sẽ sửa makeflie thành:   
```bash
.PHONY: clean
clean:
	rm -f *.exe *.o
```  
Một số chuẩn PHONY hay dùng 
```bash
| Target    | Chức năng                                                      |
|-----------|----------------------------------------------------------------|
| all       | Thực hiện build toàn bộ                                        |
| install   | Tạo bản cài đặt của ứng dụng từ việc compile binary            |
| clean     | Xóa binary file được tạo từ source                             |
| distclean | Xóa tất cả các file được tạo ra mà không nằm trong source chính |
| TAGS      | Tạo bảng tag để editor dùng                                   |
| info      | Tạo GNU info file từ textinfo source                          |
| check     | Chạy bất kỳ test nào tương ứng với chương trình               |
| build     | Biên dịch hoặc xây dựng project               				|
| run	    | Chạy chương trình								             	|
```  

# VPATH và CFLAGS trong Makefile
từ trước tới giờ các file .c .h đang để trong cùng 1 thư mục nên thao tác rát đơn giản.  
Khi để các file .c trong 1 folder và các file .h trong 1 folder![alt text](image-1.png)  
Giải pháp là:  
```bash
CFLAGS= I inc
VPATH = src
``` 

Viết vào trong Makefile

# FOREACH trong makefile 
 Nếu có nhiều folder chứa file .h nhiều folder chứa file .c thì làm như nào.  ![alt text](image-2.png)  
 Đây là lúc chúng ta dùng FOREACH: 
```bash
$(foreach var, list, text)
``` 
trong đó:  
* var là biến tạm lưu phần tử trong list trong mỗi vòng lặp
* list: danh sách các phần tử mà bạn muốn lặp qua
* text: Phần tử hoặc biểu thức cần thực hiện với mỗi phần tử trong list  
```bash
CC      := gcc

INC_DIR := ./inc ./core
SRC_DIR := ./src ./core

CFLAGS  := $(foreach d, $(INC_DIR), -I$(d))

VPATH   := $(foreach d, $(SRC_DIR), $(d))

%.o : %.c
	$(CC) -c $(CFLAGS) $< -o $@

all: main.o lib.o core.o
	$(CC) main.o lib.o core.o -o main.exe
.PHONY: clean all
clean:
	rm -f *.o *.exe

``` 

Giải thích:  
* foreach src, $(CORE_SRCS), $(src:.c=.o) nghĩa là duyệt qua từng file trong CORE_SRCS, chuyển phần mở rộng .c thành .o.

* Tương tự cho SRC_SRCS.

* Cuối cùng gom lại thành biến OBJS để biên dịch và link.