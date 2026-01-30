CC = gcc
SDKROOT = /Library/Developer/CommandLineTools/SDKs/MacOSX.sdk
CFLAGS = -isysroot $(SDKROOT) -std=gnu89 -O2 -w -Wno-error \
	 -Wno-implicit-int \
	 -Wno-implicit-function-declaration \
	 -Wno-return-type \
	 -Wno-int-conversion \
	 -Wno-incompatible-pointer-types \
	 -Wno-builtin-declaration-mismatch

install:	cg68
	cp cg68 /usr/bin/tud/cg68

cg68:	cgy.tab.o grinc.o
	$(CC) $(CFLAGS) -o cg68 cgy.tab.o grinc.o -ly -ll
	:

cgy.tab.o: cgy.tab.c lex.yy.c grinc.h
	   $(CC) -c $(CFLAGS) cgy.tab.c

cgy.tab.c: gr.y
	 cgyacc -l gr.y

lex.yy.c: gr.l
	  lex gr.l

grinc.o: grinc.c
	$(CC) -c $(CFLAGS) grinc.c

