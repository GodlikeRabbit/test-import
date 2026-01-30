SHELL	= /bin/sh
INCLUDE = ../view
CC	= gcc
SDKROOT	= /Library/Developer/CommandLineTools/SDKs/MacOSX.sdk
CFLAGS	= -isysroot $(SDKROOT) -std=gnu89 -O2 -w -Wno-error \
	  -Wno-implicit-int \
	  -Wno-implicit-function-declaration \
	  -Wno-return-type \
	  -Wno-int-conversion \
	  -Wno-incompatible-pointer-types \
	  -Wno-builtin-declaration-mismatch \
	  -c -I${INCLUDE} $(DEBUG)
YACC	= /usr/bin/yacc
DEBUG	= 

all:	adaparse adadep

install:	all
		cp adaparse adadep /usr/bin/

cleanup:
		-rm adaparse adadep *.o adadeplex.c adadepyacc.c adaparselex.c adaparseyacc.c

adaparse:	adaparseyacc.o adaparse.o
		${CC} adaparse.o adaparseyacc.o -o adaparse -ll

adadep:		adadepyacc.o adadep.o adadep1.o adadep2.o adadep3.o
		${CC} adadep.o adadepyacc.o adadep1.o adadep2.o adadep3.o ../view/viewlib.a -o adadep -ll

adaparseyacc.o:	adaparselex.c adaparseyacc.c
		${CC} ${CFLAGS} adaparseyacc.c

adadepyacc.o:	adadeplex.c adadepyacc.c adadep.h
		${CC} ${CFLAGS} adadepyacc.c

adadep1.o:	adadep.h

adadep2.o:	adadep.h

adadep3.o:	adadep.h


