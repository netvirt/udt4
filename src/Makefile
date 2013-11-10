ifndef CXX
    CXX = g++
endif

ifndef os
   os = LINUX
endif

ifndef arch
   arch = IA32
endif

CCFLAGS = -g -fPIC -Wall -pthread -Wextra -D$(os) #-finline-functions -O3 -fno-strict-aliasing #-msse3
ifeq ($(os), WIN32)
CCFLAGS = -g -fPIC -Wall -lpthread -Wextra -D$(os) -DUDT_EXPORTS -D_WIN32_WINNT=0x0501
endif

ifeq ($(arch), IA32)
   CCFLAGS += -DIA32
endif

ifeq ($(arch), POWERPC)
   CCFLAGS += -mcpu=powerpc
endif

ifeq ($(arch), SPARC)
   CCFLAGS += -DSPARC
endif

ifeq ($(arch), IA64)
   CCFLAGS += -DIA64
endif

ifeq ($(arch), AMD64)
   CCFLAGS += -DAMD64
endif

OBJS = md5.o common.o window.o list.o buffer.o packet.o channel.o queue.o ccc.o cache.o core.o epoll.o api.o
DIR = $(shell pwd)

ifeq ($(os), WIN32)
all: libudt udt
else
all: libudt libudt.a udt
endif

%.o: %.cpp %.h udt.h
	$(CXX) $(CCFLAGS) $< -c

libudt: $(OBJS)
ifeq ($(os), LINUX)
	$(CXX) -shared -o libudt.so $^
endif
ifeq ($(os), OS_X)
	$(CXX) -dynamiclib -o libudt.dylib -lstdc++ -lpthread -lm $^
endif
ifeq ($(os), WIN32)
	$(CXX) -shared -o libudt.dll $^ -Wl,--out-implib,libudt_dll.a -lws2_32
endif

libudt.a: $(OBJS)
	ar -rcs $@ $^

udt:
	cp udt.h udt

clean:
	rm -f *.o *.so *.dylib *.a udt

install:
	export LD_LIBRARY_PATH=$(DIR):$$LD_LIBRARY_PATH
