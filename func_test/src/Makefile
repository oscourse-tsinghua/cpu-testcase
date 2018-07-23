ver = test_hw
#ver = sim

# Escape the space in dir path
TOPDIR=$(shell pwd | sed 's: :\\ :g')

export LD_PRELOAD =
CFLAGS := -EL -D_KERNEL -DCONFIG_PAGE_SIZE_16KB -fno-builtin -mips32r2 -msoft-float -DCACHELOCK_MEM -DMEMSTART=0x80000000 -DMEMSIZE=0x0200000 -DCPU_COUNT_PER_US=10 -I $(TOPDIR)/include -include common.h
#CFLAGS += -fno-reorder-blocks -fno-reorder-funcions

ifeq ($(ver), sim)
CFLAGS += -DSIM
endif


export TOPDIR AR CFLAGS

export CROSS_COMPILE ?= mipsel-linux-gnu-

all: main.bin

libinst.a:
	make -C inst $(TOPDIR)/$@

%.elf: start.o libinst.a bin.lds
	${CROSS_COMPILE}ld -EL -g -T  bin.lds  -o $@ start.o -L . -linst
	rm libinst.a

dump.s: main.elf
	${CROSS_COMPILE}objdump -alD $< > $@

bin.lds: bin.lds.S
	${CROSS_COMPILE}gcc -E -P -Umips -D_LOADER -U_MAIN $(CFLAGS) bin.lds.S -o bin.lds

main.bin:main.elf
	${CROSS_COMPILE}objcopy -O binary -j .text -j .data $< $@ 

clean:
	rm -f *.o *.bin *.data *.elf *.a *.s *.vlog bin.lds
	make -C inst clean

-include rules.make
