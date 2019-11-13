TAISH_DIR := sm/oopt-tai/tools/taish/

.DEFAULT_GOAL := all

INCLUDE := -I sm/oopt-tai/inc

all: main

LIB := `pkg-config --libs protobuf grpc++ grpc`

OBJS := tai.grpc.pb.o tai.pb.o main.o taiclient.o

CC := g++
CFLAGS := -std=c++11

.cc.o:
	$(CC) $(CFLAGS) $(INCLUDE) -c -o $@ $<

.cpp.o:
	$(CC) $(CFLAGS) $(INCLUDE) -c -o $@ $<

GRPC_FILES := tai.pb.cc tai.pb.h tai.grpc.pb.cc tai.grpc.pb.h

$(GRPC_FILES): $(TAISH_DIR)/proto/tai.proto
	protoc --cpp_out=. -I $(TAISH_DIR)/proto tai.proto
	protoc --grpc_out=. --plugin=protoc-gen-grpc=`which grpc_cpp_plugin` -I $(TAISH_DIR)proto tai.proto

main: $(GRPC_FILES) Makefile $(OBJS)
	g++ -g -std=c++11 -I /usr/local/include/sysrepo -o main $(OBJS) -lyang-cpp -lSysrepo-cpp $(LIB) -lgrpc++_reflection -lpthread -lyang -lsysrepo

clean:
	$(RM) $(GRPC_FILES) main *.o