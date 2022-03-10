load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")
http_archive(
    name = "build_bazel_rules_nodejs",
    sha256 = "965ee2492a2b087cf9e0f2ca472aeaf1be2eb650e0cfbddf514b9a7d3ea4b02a",
    urls = ["https://github.com/bazelbuild/rules_nodejs/releases/download/5.2.0/rules_nodejs-5.2.0.tar.gz"],
)

load("@build_bazel_rules_nodejs//:repositories.bzl", "build_bazel_rules_nodejs_dependencies")

build_bazel_rules_nodejs_dependencies()

load("@build_bazel_rules_nodejs//:index.bzl", "node_repositories")
node_repositories(
)

load("@rules_nodejs//nodejs:repositories.bzl", "nodejs_register_toolchains")

nodejs_register_toolchains(
    name = "nodejs",
    node_version = "17.6.0",
)

http_archive(
    name = "vendored_yarn",
    build_file_content = """exports_files(["bin/yarn.js"])""",
    sha256 = "267982c61119a055ba2b23d9cf90b02d3d16c202c03cb0c3a53b9633eae37249",
    strip_prefix = "yarn-v1.22.17",
    urls = ["https://github.com/yarnpkg/yarn/releases/download/v1.22.17/yarn-v1.22.17.tar.gz"],
)


load("@build_bazel_rules_nodejs//:index.bzl", "yarn_install")
yarn_install(
    name = "npm",
    package_json = "//vuepress-starter:package.json",
    symlink_node_modules = False,
    use_global_yarn_cache = True,
    yarn_lock = "//vuepress-starter:yarn.lock",
)

http_archive(
    name = "io_bazel_rules_docker",
    sha256 = "85ffff62a4c22a74dbd98d05da6cf40f497344b3dbf1e1ab0a37ab2a1a6ca014",
    strip_prefix = "rules_docker-0.23.0",
    urls = ["https://github.com/bazelbuild/rules_docker/releases/download/v0.23.0/rules_docker-v0.23.0.tar.gz"],
)

register_toolchains(
    "@io_bazel_rules_docker//toolchains/docker:default_linux_toolchain",
    "@io_bazel_rules_docker//toolchains/docker:default_windows_toolchain",
    "@io_bazel_rules_docker//toolchains/docker:default_osx_toolchain",
)

load(
    "@io_bazel_rules_docker//repositories:repositories.bzl",
    container_repositories = "repositories",
)

container_repositories()

load("@io_bazel_rules_docker//repositories:deps.bzl", container_deps = "deps")

container_deps()

load(
    "@io_bazel_rules_docker//container:container.bzl",
    "container_pull",
)

container_pull(
    name = "nodejs_image_base",
    digest = "sha256:cda2b5cc143a59ec47e6d95ef2110a5531b1dc0f5bc42017a10b381042b8d629",
    registry = "gcr.io",
    repository = "google-appengine/debian9",
)