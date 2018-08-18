cmake_minimum_required(VERSION 3.10.0)
project(route_guide)

set(CMAKE_CXX_FLAGS, "${CMAKE_CXX_FLAGS} -std=c++11")

set(protobuf_MODULE_COMPATIBLE TRUE)

# Openssl 
find_package(OpenSSL MODULE REQUIRED)
message(STATUS "Using openssl version ${OpenSSL_VERSION}")

if(TARGET OpenSSL::SSL)
 set(_OPENSSL_LIBRARIES OpenSSL::SSL OpenSSL::Crypto)
else()
 set(_OPENSSL_LIBRARIES ${OPENSSL_LIBRARIES})
endif()
set(_OPENSSL_INCLUDE_DIR ${OPENSSL_INCLUDE_DIR})

# Search for protobuf library in local computer.
find_package(Protobuf CONFIG REQUIRED)
message(STATUS "Using Protobuf version ${Protobuf_VERSION}")

# Library of protobuf for linking codes.
set(_PROTOBUF_LIBPROTOBUF protobuf::libprotobuf)
# Executable protoc for compling the proto files.
set(_PROTOBUF_PROTOC $<TARGET_FILE:protobuf::protoc>)

# Search for grpc library in local computer.
find_package(gRPC CONFIG REQUIRED)
message(STATUS "Using gRPC version ${gRPC_VERSION}") 

# Library of gRPC for linking codes.
set(_GRPC_GRPCPP_INSECURE gRPC::grpc++)
set(_GRPC_CPP_PLUGIN_EXECUTABLE $<TARGET_FILE:gRPC::grpc_cpp_plugin>)

# Proto file.
get_filename_component(route_guide_proto "./protos/route_guide.proto" ABSOLUTE)
get_filename_component(route_guide_proto_path "${route_guide_proto}" PATH)

# Generated sources.
set(rg_proto_hdr "${CMAKE_CURRENT_SOURCE_DIR}/route_guide.pb.h")
set(rg_proto_src "${CMAKE_CURRENT_SOURCE_DIR}/route_guide.pb.cc")
set(rg_grpc_hdr "${CMAKE_CURRENT_SOURCE_DIR}/route_guide.grpc.pb.h")
set(rg_grpc_src "${CMAKE_CURRENT_SOURCE_DIR}/route_guide.grpc.pb.cc")

# Generate proto and grpc cpp files.
add_custom_command(
 OUTPUT "${rg_proto_hdr}" "${rg_proto_src}" "${rg_grpc_hdr}" "${rg_grpc_src}"
 COMMAND ${_PROTOBUF_PROTOC}
 ARGS --grpc_out "${CMAKE_CURRENT_SOURCE_DIR}"
 			--cpp_out "${CMAKE_CURRENT_SOURCE_DIR}"
			-I "${route_guide_proto_path}"
			--plugin=protoc-gen-grpc="${_GRPC_CPP_PLUGIN_EXECUTABLE}"
			"${route_guide_proto}"
 DEPENDS "${route_guide_proto}"
)

# Add the generate header files.
include_directories("${CMAKE_CURRENT_SOURCE_DIR}" "${OPENSSL_INCLUDE_DIR}")

# Generate the targets: route_guide_server and route_guide_client
foreach (_target route_guide_client route_guide_server)
 add_executable(${_target}
	"${CMAKE_CURRENT_SOURCE_DIR}/${_target}.cc"
	${rg_grpc_src}
  ${rg_proto_src}
	"${CMAKE_CURRENT_SOURCE_DIR}/helper.cc")

 target_link_libraries(${_target}
	${_OPENSSL_LIBRARIES}
 	${_GRPC_GRPCPP_INSECURE}
  ${_PROTOBUF_LIBPROTOBUF})
endforeach(_target)
