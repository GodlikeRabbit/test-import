SHELL	= /bin/sh
LIB	= /home/adacomp/lib
B	= /bin/cp
A	= /bin/cat
F	= ada_front
CG	= ada_cg
CC	= gcc
SDKROOT	= /Library/Developer/CommandLineTools/SDKs/MacOSX.sdk
CFLAGS	= -isysroot $(SDKROOT) -std=gnu89 -O2 -w -Wno-error \
	  -Wno-implicit-int \
	  -Wno-implicit-function-declaration \
	  -Wno-return-type \
	  -Wno-int-conversion \
	  -Wno-incompatible-pointer-types \
	  -Wno-builtin-declaration-mismatch

all:	std.a std_ps.t

std_ps.o:	std_ps.s
	cp std_ps.s std_ps.o

std_ps.s:	std_ps.p
	cat < std_ps.p > std_ps.s

std_ps.t std_ps.p:	std_ps.ada
	-${LIB}/${F}  -n std_ps.ada
	-${LIB}/${CG} std_ps.t std_ps.p 2 std_ps.f

std_pb.o:	std_pb.c
	$(CC) $(CFLAGS) -c std_pb.c

cleanup:
	rm -f std_ps.s std_ps.p std_ps.t std_ps.f std_ps.o std.a

install:	all
	cp std.a ${LIB}/std.a
	cp std_ps.t ${LIB}/std_ps.t
	cp std_ps.o ${LIB}/std_ps.o

std.a:	std_ps.o 
	ar r std.a std_ps.o
