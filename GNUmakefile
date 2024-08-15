include ../Makefile.crt

LIBC_OBJS  = $(patsubst %.c,%.o,$(wildcard *.c))
LIBC_OBJS += $(patsubst %.c,%.o,$(wildcard */*.c))
LIBC_OBJS += $(patsubst %.c,%.o,$(wildcard arch/$(KARCH)/*.c))

KARCH = x86_64
CC = $(KARCH)-elf-gcc
CFLAGS := -Os -std=gnu11 -ffreestanding -Wall -Wextra -Wno-unused-parameter -I../base/usr/include

AR = $(KARCH)-elf-ar

LC = ../base/lib/libc.so $(GCC_SHARED)
GCC_SHARED = ../base/usr/lib/libgcc_s.so.1 ../base/usr/lib/libgcc_s.so

.PHONY: all
all: $(BASE)/lib/crt%.o ../base/lib/libc.a ../base/lib/libc.so ../base/lib/libm.so

$(BASE)/lib/crt%.o: arch/$(KARCH)/crt%.S
        @echo -e 'AS' $@
        @$(AS) -o $@ $<

%.o: %.c ../base/usr/include/syscall.h
	@echo -e 'CC' $@
	@$(CC) $(CFLAGS) -fPIC -c -o $@ $<

../base/lib/libc.a: $(LIBC_OBJS) $(CRTS)
	@echo -e 'AR' $@
	@$(AR) cr $@ $(LIBC_OBJS)

../base/lib/libc.so: $(LIBC_OBJS) | $(CRTS)
	@echo -e 'CC' $@
	@$(CC) -nodefaultlibs -shared -fPIC -o $@ $^ -lgcc

../base/lib/libm.so: ../util/libm.c
	@echo -e 'CC' $@
	@$(CC) -shared -nostdlib -fPIC -o $@ $<

.PHONY: clean
clean:
	@rm -rf *.o */*.o *.so *.a ../base/lib/*.a ../base/lib/*.o ../base/lib/*.so
