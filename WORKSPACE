http_archive (
	name = "com_github_protobuf",
	urls = [
		"https://github.com/google/protobuf/archive/48cb18e5c419ddd23d9badcfe4e9df7bde1979b2.tar.gz",
		],
	strip_prefix = "protobuf-48cb18e5c419ddd23d9badcfe4e9df7bde1979b2",
)

http_archive (
	name = "com_github_grpc_grpc",
	urls = [
		"https://github.com/grpc/grpc/archive/d8020cb6daa87f1a3bb3b0c299bc081c4a3de1e8.tar.gz",
		],
	strip_prefix = "grpc-d8020cb6daa87f1a3bb3b0c299bc081c4a3de1e8",
)

load("@com_github_grpc_grpc//bazel:grpc_deps.bzl", "grpc_deps")

grpc_deps()
