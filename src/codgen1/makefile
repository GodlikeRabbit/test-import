#ms68
#19880715/wjvg

CC = gcc
SDKROOT = /Library/Developer/CommandLineTools/SDKs/MacOSX.sdk
CFLAGS = -isysroot $(SDKROOT) -std=gnu89 -O2 -w -Wno-error \
	 -Wno-implicit-int \
	 -Wno-implicit-function-declaration \
	 -Wno-return-type \
	 -Wno-int-conversion \
	 -Wno-incompatible-pointer-types \
	 -Wno-builtin-declaration-mismatch \
	 -Df68000 -Don68000

s68:	c.o d.o def68.o defs68.o hnode.o hsize.o i.o m.o o.o opz.o p.o \
	  s.o x68z.o extra.o \
	dupstr.o	\
	roct.o		\
	index.o		\
	rdec.o		\
	rdecl.o		\
	eopti.o		\
	eopen.o		\
	enuexp.o
	$(CC) $(CFLAGS) -o s68 \
	  c.o d.o def68.o defs68.o hnode.o hsize.o i.o m.o o.o opz.o p.o \
	  s.o x68z.o extra.o \
	dupstr.o	\
	roct.o		\
	index.o		\
	rdec.o		\
	rdecl.o		\
	eopti.o		\
	eopen.o		\
	enuexp.o

.c.o:
	$(CC) -c $(CFLAGS) $*.c

install:	s68
	cp s68 /usr/bin/tud

#end
