# all: main

# main: main.c lib.c
# 	gcc main.c lib.c -o main.exe

# Rules make file

# main.o: main.c 
# 	gcc -c main.c -o main.o
# lib.o: lib.c
# 	gcc -c lib.c -o lib.o
# all: main.o lib.o
# 	gcc main.o lib.o -o main.exe
# clean:
# 	rm -f *.exe *.o

# Variables make file
CC = gcc
CFLAGS = -c
SRCS = main.c lib.c
OBJS = main.o lib.o
TARGET = main.exe

all: $(TARGET)

$(TARGET): $(OBJS)
	$(CC) $(OBJS) -o $(TARGET)

%.o: %.c
	$(CC) $(CFLAGS) $< -o $@

.PHONY: clean
clean:
	rm -f $(OBJS) $(TARGET)