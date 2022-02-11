TARGET := ab

CC ?= cc
RM ?= rm -f
INSTALL ?= install
GCOV ?= gcov

CFLAGS += -std=c11 -Wall $(COVERAGE_CFLAGS)

SRCS := $(wildcard *.c)
OBJS := $(SRCS:.c=.o)
DEPS := $(OBJS:.o=.d)

PREFIX ?= /usr/local
INSTALL_BIN := $(PREFIX)/bin

all: $(TARGET)

%.o: %.c
	$(CC) $(CFLAGS) -MMD -MP -c -o $@ $<

$(TARGET) : $(OBJS)
	$(CC) $(CFLAGS) $(LDFLAGS) $^ -o $@

test: all
	(cd ..; ./runtest)

coverage:
	$(MAKE) test COVERAGE_CFLAGS="-coverage"
	$(GCOV) *.c

clean:
	$(RM) $(TARGET) *.d *.o *.gcda *.gcno *.gcov

install: all
	$(INSTALL) -m 0755 -d $(INSTALL_BIN)
	$(INSTALL) -m 0755 $(TARGET) $(INSTALL_BIN)

uninstall:
	$(RM) $(INSTALL_BIN)/$(TARGET)

-include $(DEPS)

.PHONY: all test coverage clean install uninstall
