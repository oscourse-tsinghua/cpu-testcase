srcs=$(wildcard *.c)
objs=$(srcs:.c=.o) 

libtinyc.a: $(objs)
	$(CROSS_COMPILE)$(AR) -cr $@ $^

clean:
	rm -f *.o *.a *.s

%.o: %.c
	${CROSS_COMPILE}gcc $(CFLAGS) -g -DGUEST -I ../include -I .  -c $<
