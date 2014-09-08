ifndef CXX
    CXX = g++
endif

ifndef os
   os = LINUX
endif

ifndef arch
   arch = IA32
endif

CCFLAGS = -g -fPIC -Wall -Wextra -D$(os) #-finline-functions -O3 -fno-strict-aliasing #-msse3

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


ifeq ($(os), OSX)
   SHARED_LIB = libudt.dylib
   STATIC_LIB = libudt.a
   MACOS_SDK_SELECT = -mmacosx-version-min=10.6 -isysroot /SDKs/MacOSX10.6.sdk
   CCFLAGS += $(MACOS_SDK_SELECT)
   LDFLAGS += $(MACOS_SDK_SELECT) -dynamiclib -o libudt.dylib -lstdc++ -lpthread -lm
else ifeq ($(os), WIN32)
   SHARED_LIB = libudt.dll
   STATIC_LIB =
   CCFLAGS += -lpthread -DUDT_EXPORTS -D_WIN32_WINNT=0x0501
   LDFLAGS += -shared -Wl,--out-implib,libudt_dll.a -lws2_32
else
   SHARED_LIB = libudt.so
   STATIC_LIB = libudt.a
   LDFLAGS += -shared
endif

OBJS = md5.o common.o window.o list.o buffer.o packet.o channel.o queue.o ccc.o cache.o core.o epoll.o api.o
DIR = $(shell pwd)

all: $(SHARED_LIB) $(STATIC_LIB) udt

%.o: %.cpp %.h udt.h
	$(CXX) $(CCFLAGS) $< -c

$(SHARED_LIB): $(OBJS)
	$(CXX) -o $@ $^ $(LDFLAGS)

libudt.a: $(OBJS)
	ar -rcs $@ $^

udt:
	cp udt.h udt

clean:
	rm -f *.o *.so *.dylib *.a udt

install:
	export LD_LIBRARY_PATH=$(DIR):$$LD_LIBRARY_PATH
