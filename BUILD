cc_library (
	name = "route_guide_proto_base",
	srcs = [
		"route_guide.grpc.pb.cc",
		"route_guide.pb.cc"
		],
	hdrs = [
		"route_guide.grpc.pb.h",
		"route_guide.pb.h",
		],
	deps = [
		"@com_github_protobuf//:protobuf",
		"@com_github_grpc_grpc//:grpc++",
		],
)


cc_library (
	name = "route_guide_helper",
	srcs = [
		"helper.cc",
		],
	hdrs = [
		"helper.h",
		],
	deps = [
		":route_guide_proto_base",
		],
)


cc_binary (
  name = "route_guide_server",
	srcs = [
		"route_guide.pb.h",
		"route_guide.grpc.pb.h",
		"helper.h",
		"route_guide_server.cc",
		],
	deps = [
		":route_guide_proto_base",
		":route_guide_helper",
		"@com_github_grpc_grpc//:grpc++",
		"@com_github_protobuf//:protobuf",
		],
)


cc_binary (
	name = "route_guide_client",
	srcs = [
		"route_guide.pb.h",
		"route_guide.grpc.pb.h",
		"helper.h",
		"route_guide_client.cc",
		],
	deps = [
		":route_guide_proto_base",
		":route_guide_helper",
		"@com_github_grpc_grpc//:grpc++",
		"@com_github_protobuf//:protobuf",
		],
)
