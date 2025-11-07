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
gcc main.c lib.c -o main.exe -I
 ./mainexe  
```
