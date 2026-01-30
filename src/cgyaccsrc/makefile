CC = gcc
SDKROOT = /Library/Developer/CommandLineTools/SDKs/MacOSX.sdk
CFLAGS = -isysroot $(SDKROOT) -std=gnu89 -O2 -w -Wno-error \
	 -Wno-implicit-int \
	 -Wno-implicit-function-declaration \
	 -Wno-return-type \
	 -Wno-int-conversion \
	 -Wno-incompatible-pointer-types \
	 -Wno-builtin-declaration-mismatch

install:	cgyacc
	cp cgyacc /usr/bin/tud/cgyacc
	:

cgyacc: y1.o y2.o y3.o y4.o y5.o y6.o y7.o y8.o
	$(CC) $(CFLAGS) -o cgyacc y1.o y2.o y3.o y4.o y5.o y6.o y7.o y8.o

y1.o y2.o y3.o y4.o y5.o y6.o y7.o y8.o: dextern files
y2.o y5.o y6.o: prep.h
