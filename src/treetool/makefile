# makefile for treetool
# veyacc version
SHELL	= /bin/sh
CC	= gcc
YACC	= ../veyacc/veyacc

SDKROOT	= /Library/Developer/CommandLineTools/SDKs/MacOSX.sdk
CFLAGS	= -isysroot $(SDKROOT) -std=gnu89 -O2 -w -Wno-error \
	  -Wno-implicit-int \
	  -Wno-implicit-function-declaration \
	  -Wno-return-type \
	  -Wno-int-conversion \
	  -Wno-incompatible-pointer-types \
	  -Wno-builtin-declaration-mismatch
LIBS	= -ll


treetool:	y.tab.c lex.yy.c
	$(CC) $(CFLAGS) -o treetool y.tab.c $(LIBS)

y.tab.c:	treegen.y
	$(YACC) treegen.y

lex.yy.c:	treegen.l
	lex treegen.l

