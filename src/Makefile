CC=gcc
CXX=g++
CFLAGS=-Wall -I ${HOME}/include
CXXFLAGS=-std=c++0x -Wall -I ${HOME}/include
LDFLAGS=
AR=ar
ARFLAGS=crs
TARGET = ${HOME}/lib/libcommon.a
MYSQLLIBS = -lmysqlpp
MYSQLINCLUDE = -I /usr/include/mysql++ -I /usr/include/mysql
CXXFLAGS += $(MYSQLINCLUDE)
LDFLAGS += $(MYSQLLIBS)

.DEFAULT: all
.PHONY: clean

SRCS = $(wildcard *.cc) $(wildcard *.c)
HDRS = $(SRCS:.cc=.h) $(SRCS:.c=.h)
OBJS = $(SRCS:.cc=.o) $(SRCS:.c=.o)

all: $(TARGET)

$(TARGET):	$(OBJS)
	$(AR) $(ARFLAGS) $@ $?

clean:
	rm -rf $(TARGET) *~ *.o

