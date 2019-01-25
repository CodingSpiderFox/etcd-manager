load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive", "http_file")

http_archive(
    name = "io_bazel_rules_go",
    url = "https://github.com/bazelbuild/rules_go/releases/download/0.16.6/rules_go-0.16.6.tar.gz",
    sha256 = "ade51a315fa17347e5c31201fdc55aa5ffb913377aa315dceb56ee9725e620ee",
)

http_archive(
    name = "bazel_gazelle",
    urls = ["https://github.com/bazelbuild/bazel-gazelle/releases/download/0.16.0/bazel-gazelle-0.16.0.tar.gz"],
    sha256 = "7949fc6cc17b5b191103e97481cf8889217263acf52e00b560683413af204fcb",
)

load("@io_bazel_rules_go//go:def.bzl", "go_rules_dependencies", "go_register_toolchains")

go_rules_dependencies()

go_register_toolchains(
    go_version = "1.11.5",
)

load("@bazel_gazelle//:deps.bzl", "gazelle_dependencies", "go_repository")

gazelle_dependencies()

#=============================================================================

http_archive(
    name = "io_bazel_rules_docker",
    sha256 = "c0e9d27e6ca307e4ac0122d3dd1df001b9824373fb6fb8627cd2371068e51fef",
    strip_prefix = "rules_docker-0.6.0",
    urls = ["https://github.com/bazelbuild/rules_docker/archive/v0.6.0.tar.gz"],
)

load(
    "@io_bazel_rules_docker//container:container.bzl",
    "container_pull",
    container_repositories = "repositories",
)

container_repositories()

# Use a kubernetes base image which includes ca-certificates (so we can talk to google for OAuth)
# Some potential here: https://github.com/GoogleCloudPlatform/base-images-docker
container_pull(
    name = "debian_base_amd64",
    digest = "sha256:cc782ed16599000ca4c85d47ec6264753747ae1e77520894dca84b104a7621e2",
    registry = "gcr.io",
    repository = "google-containers/debian-hyperkube-base-amd64",
    tag = "0.10",
)

#=============================================================================
# etcd rules
http_file(
    name = "etcd_2_2_1_tar",
    sha256 = "59f7985c81b6bc551246c165c2fd83e33a063875e4e0c61920b1d90a4910f462",
    urls = ["https://github.com/coreos/etcd/releases/download/v2.2.1/etcd-v2.2.1-linux-amd64.tar.gz"],
)

http_file(
    name = "etcd_3_1_12_tar",
    sha256 = "4b22184bef1bba8b4908b14bae6af4a6d33ec2b91e4f7a240780e07fa43f2111",
    urls = ["https://github.com/coreos/etcd/releases/download/v3.1.12/etcd-v3.1.12-linux-amd64.tar.gz"],
)

http_file(
    name = "etcd_3_2_18_tar",
    sha256 = "b729db0732448064271ea6fdcb901773c4fe917763ca07776f22d0e5e0bd4097",
    urls = ["https://github.com/coreos/etcd/releases/download/v3.2.18/etcd-v3.2.18-linux-amd64.tar.gz"],
)

http_file(
    name = "etcd_3_2_24_tar",
    sha256 = "947849dbcfa13927c81236fb76a7c01d587bbab42ab1e807184cd91b026ebed7",
    urls = ["https://github.com/coreos/etcd/releases/download/v3.2.24/etcd-v3.2.24-linux-amd64.tar.gz"],
)

#=============================================================================
# Build etcd from source
# This picks up a number of critical bug fixes, for example:
#  * Caching of /etc/hosts https://github.com/golang/go/issues/13340
#  * General GC etc improvements
#  * Misc security fixes that are not backported

# Download via HTTP
go_repository(
    name = "etcd_v2_2_1_source",
    urls = ["https://github.com/etcd-io/etcd/archive/v2.2.1.tar.gz"],
    sha256 = "1c0ce63812ef951f79c0a544c91f9f1ba3c6b50cb3e8197de555732454864d05",
    importpath = "github.com/coreos/etcd",
    strip_prefix = "etcd-2.2.1/",
    build_external = "vendored",
    build_file_proto_mode = "disable_global",
)
