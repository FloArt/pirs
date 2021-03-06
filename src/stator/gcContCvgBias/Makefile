ifeq ($(shell uname -s),Darwin)
CPPFLAGS += -march=core2 -Bstatic -L/opt/local/lib -I/opt/local/include
else
CPPFLAGS += -static
endif

GCC_VERSION := $(shell gcc -dumpversion)
GCC_MAJOR := $(shell echo $(GCC_VERSION) | cut -d'.' -f1)
GCC4_VER := $(shell echo $(GCC_VERSION) | cut -d'.' -f2,3)

ifeq ($(GCC_MAJOR),4)
ifeq ($(shell expr 2.4 \<= $(GCC4_VER)),1)
CPPFLAGS += -march=native
endif
ifeq ($(shell expr 3.6 \<= $(GCC4_VER)),1)
CPPFLAGS += -std=c++0x
endif
endif


OBJ = gzstream.o self_util.o main.o stat_soap_coverage.o
CC = g++
LIBRARY = -O3 -pipe $(CPPFLAGS) -mtune=generic -pedantic -Wfloat-equal \
#-Wall

gc_coverage_bias:${OBJ}
	${CC} -o gc_coverage_bias ${LIBRARY} ${OBJ} -lz
gzstream.o:gzstream.h gzstream.cpp
	${CC} -c ${LIBRARY} gzstream.h gzstream.cpp
self_util.o:self_util.cpp self_util.h
	${CC} -c ${LIBRARY} self_util.cpp self_util.h
main.o:main.cpp
	${CC} -c ${LIBRARY} main.cpp
stat_soap_coverage.o:stat_soap_coverage.h stat_soap_coverage.cpp
	${CC} -c ${LIBRARY} stat_soap_coverage.h stat_soap_coverage.cpp

test: gc_coverage_bias
	./gc_coverage_bias -r gcdeptestref.fa.gz -o test -w 100,200 --gcdump --depwindump gcdeptest.depth.gz
	-./gc_coverage_bias_plot.pl test_100.dat
	-./gc_coverage_bias_plot.pl test_200.dat
	ls -l test*

distclean: compclean
	-rm -v test_*

compclean:
	-rm -v ${OBJ} *.gch

clean: distclean
	-rm gc_coverage_bias
