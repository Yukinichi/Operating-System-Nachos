# GNU Makefile for building user programs to run on top of Nachos
#
# Things to be aware of:
#
#    The value of the ARCHDIR environment variable must be set before using
#	this makefile. If you are using an instructional machine, this should
#	be automatic. However, if you are not using an instructional machine,
#	you need to point ARCHDIR at the cross-compiler directory, e.g.
#		setenv ARCHDIR ../mips-x86.win32-xgcc

# you need to point to the right executables
GCCDIR = $(ARCHDIR)/mips-

ASFLAGS = -mips1
CPPFLAGS =
CFLAGS = -O2 -B$(GCCDIR) -G 0 -Wa,-mips1 -nostdlib -ffreestanding -std=gnu99
LDFLAGS = -s -T script -N -warn-common -warn-constructors -warn-multiple-gp

CC = $(GCCDIR)gcc
AS = $(GCCDIR)as
LD = $(GCCDIR)ld
CPP = $(GCCDIR)cpp
AR = $(GCCDIR)ar
RANLIB = $(GCCDIR)ranlib

STDLIB_H = stdio.h stdlib.h ag.h
STDLIB_C = stdio.c stdlib.c
STDLIB_O = start.o stdio.o stdlib.o

LIB = assert atoi printf readline stdio strncmp strcat strcmp strcpy strlen memcpy memset rand
NLIB = libnachos.a

TARGETS = halt sh matmult sort echo cat cp mv rm creat1 creat2 creat3 creat4 open1 open2 read1 read2 read3 read4 write1 write2 write3 write4 write5 write6 write10 close1 unlink exec1 join1 exit1 execarg1 page_fault join_waiting simpleTest spin_running exec2

.SECONDARY: $(patsubst %.c,%.o,$(wildcard *.c))

all: $(patsubst %,%.coff,$(TARGETS))

ag: grade-file.coff grade-exec.coff grade-mini.coff grade-dumb.coff

clean:
	rm -f strt.s *.o *.coff $(NLIB)

agclean: clean
	rm -f f1-* f2-*

$(NLIB): $(patsubst %,$(NLIB)(%.o),$(LIB)) start.o
	$(RANLIB) $(NLIB)

start.o: start.s syscall.h
	$(CPP) $(CPPFLAGS) start.s > strt.s
	$(AS) $(ASFLAGS) -o start.o strt.s
	rm strt.s

%.o: %.c *.h
	$(CC) $(CFLAGS) -c $<

%.coff: %.o $(NLIB)
	$(LD) $(LDFLAGS) -o $@ $< start.o -lnachos
