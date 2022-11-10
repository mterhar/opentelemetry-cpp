cc_binary(
    name = "example_http_client2",
    srcs = [
        "client.cc",
        "tracer_common.h",
    ],
    copts = [
        "-DWITH_CURL",
    ],
    linkopts = select({
        "//bazel:windows": [
            "-DEFAULTLIB:advapi32.lib",
            "-DEFAULTLIB:crypt32.lib",
            "-DEFAULTLIB:Normaliz.lib",
        ],
        "//conditions:default": [],
    }),
    tags = ["grpc"],
    deps = [
        "//api",
        "//exporters/otlp:otlp_http_exporter",
        "//ext:headers",
        "//ext/src/http/client/curl:http_client_curl",
        "//sdk/src/trace",
    ],
)

cc_binary(
    name = "example_http_server2",
    srcs = [
        "server.cc",
        "server.h",
        "tracer_common.h",
    ],
    tags = ["grpc"],
    deps = [
        "//api",
        "//exporters/otlp:otlp_http_exporter",
        "//ext:headers",
        "//sdk/src/trace",
    ],
)
