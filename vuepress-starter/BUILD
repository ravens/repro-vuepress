load("@build_bazel_rules_nodejs//:index.bzl", "copy_to_bin")
load("@npm//vuepress:index.bzl", "vuepress")
load("@npm//http-server:index.bzl", "http_server")
load("@io_bazel_rules_docker//nodejs:image.bzl", "nodejs_image")
load("@io_bazel_rules_docker//container:container.bzl", "container_image")

copy_to_bin(
    name = "vuepress_srcs",
    srcs = glob(
        include = [
            "docs/**/*",
        ],
        exclude = [
            "docs/.vuepress/.temp/**/*",
        ],
    ),
)

vuepress(
    name = "build",
    args = [
        "build",
        "docs",
        "-d $(@D)",
    ],
    chdir = "$(RULEDIR)",
    data = [
        ":vuepress_srcs",
    ],
    output_dir = True,
)

http_server(
    name = "serve",
    data = [
        ":build",
    ],
    templated_args = [
        "$$(rlocation $(location :build))",
    ],
)

nodejs_image(
    name = "nodejs_image",
    binary = ":serve" ,
    visibility = ["//visibility:public"],
    node_repository_name = "nodejs_linux_amd64",
    include_node_repo_args = False,
)

container_image(
    name = "image",
    base = ":nodejs_image",
    stamp = "@io_bazel_rules_docker//stamp:always",
    tags = [
        "manual",
    ],
)