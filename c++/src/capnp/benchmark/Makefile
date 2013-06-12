CXX=g++-mp-4.8 -std=c++11 -O2

BINARIES=capnproto-carsales capnproto-catrank capnproto-eval \
	protobuf-carsales protobuf-catrank protobuf-eval \
	null-carsales null-catrank null-eval runner

all : $(BINARIES)

.PHONY : clean all


clean :
	rm -f *.o *.pb.h *.pb.cc *.capnp.h *.capnp.c++ $(BINARIES) *-middleman

capnproto-%-middleman :
	capnpc -I../../ $*.capnp --o c++
	touch capnproto-%-middleman

capnproto-% : capnproto-%-middleman capnproto-%.c++ common.h capnproto-common.h
	$(CXX) -c -I/usr/local/include $*.capnp.c++
	$(CXX) -L/usr/local/lib -lcapnproto -I/usr/local/include capnproto-$*.c++ $*.capnp.o -o $@

protobuf-%-middleman :
	protoc $*.proto --cpp_out=./
	touch protobuf-$*-middleman

protobuf-% : protobuf-%-middleman protobuf-%.c++ common.h protobuf-common.h
	$(CXX) -c $*.pb.cc
	$(CXX) /opt/local/lib/libprotobuf.a protobuf-$*.c++ $*.pb.o -o $@

null-% : null-%.c++
	$(CXX) $< -o $@

runner : runner.c++
	$(CXX) runner.c++ -o runner
