# Makefile used to build Webots files

TARGETS = all clean
.PHONY: $(TARGETS)

CXX=g++
CC=gcc
LD=ld
STRIP=strip
CXXFLAGS= -O1 -fpic

SHLIBEXT= so
LIBOPTS= -shared -fpic 

ifndef OSTYPE
  OSTYPE = $(shell uname -s|awk '{print tolower($$0)}')
  #export OSTYPE
endif

ifeq ($(OSTYPE),linux)
  SHLIBEXT= so
  LIBOPTS= -shared -fpic
  LIBRT= -lrt
  LIBLNK = -lboost_python
  LIBNAME= -Wl,-soname,"lib$@.$(SHLIBEXT)"
endif
ifeq ($(OSTYPE),darwin)
  SHLIBEXT= dylib
  LIBOPTS= -bundle -undefined dynamic_lookup
  LIBNAME= -Wl,-install_name,"lib$@.$(SHLIBEXT)"
  LIBLNK=-lboost_python-mt
  ifeq ($(MODE),32)
    CC=gcc -arch i386
    CXX=g++ -arch i386
    LD=g++ -arch i386
  endif
  CXXFLAGS= -O2
  LIBRT=
endif

BOOST_INCLUDE_DIRS = -I/usr/local/include/boost -I/usr/local/include/boost/python

INCLUDE_DIRS = -I/usr/local/include -I/usr/include/python2.6 $(BOOST_INCLUDE_DIRS)
LIB_DIRS = -L/usr/local/lib

all: pyshm 

%.o: %.cc
	$(CXX) $(CXXFLAGS) $(INCLUDE_DIRS) -o $@ -c $<
%.o: %.cpp
	$(CXX) $(CXXFLAGS) $(INCLUDE_DIRS) -o $@ -c $<
%.o: %.c
	$(CC) $(CXXFLAGS) $(INCLUDE_DIRS) -o $@ -c $<

pyshm: pyshm.o
#	$(CXX) -Wl,-soname,"lib$@.$(SHLIBEXT)" $(LIBOPTS) $^ $(LIBRT) $(LIBLNK) -o $@.$(SHLIBEXT)
	$(CXX) $(LIBNAME) $(LIBOPTS) $^ $(LIBRT) $(LIBLNK) -o $@.$(SHLIBEXT)
clean:
	rm -f *.o *.$(SHLIBEXT) 
