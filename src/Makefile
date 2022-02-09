TARGET := ab
CC ?= cc
CFLAGS += -std=c11 -Wall

SRCS := $(wildcard *.c)
OBJS := $(SRCS:.c=.o)
DEPS := $(OBJS:.o=.d)

PREFIX ?= /usr/local
INSTALLBIN := $(PREFIX)/bin
INSTALL ?= install

all: $(TARGET)

%.o: %.c
	$(CC) $(CFLAGS) -MMD -MP -c -o $@ $<

$(TARGET) : $(OBJS)
	$(CC) $(CFLAGS) $(LDFLAGS) $^ -o $@

test: all
	(cd ..; ./runtest)

coverage:
	$(MAKE) test CFLAGS+="-coverage -O0"
	gcov *.c

clean:
	rm -f $(TARGET) *.d *.o *.gcda *.gcno *.gcov

install: all
	$(INSTALL) -m 0755 -d $(INSTALLBIN)
	$(INSTALL) -m 0755 $(TARGET) $(INSTALLBIN)

uninstall:
	rm -f $(INSTALLBIN)/$(TARGET)

-include $(DEPS)

.PHONY: all test coverage clean install uninstall
