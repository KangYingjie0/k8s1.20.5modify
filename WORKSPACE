workspace(name = "io_k8s_kubernetes")

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive", "http_file")
load("//build:workspace_mirror.bzl", "mirror")

http_archive(
    name = "io_k8s_repo_infra",
    strip_prefix = "repo-infra-0.1.4",
    sha256 = "79bc5b19ca07bf0d105a646654f0b043c7d2697aacb67bf44f1dfeb172fe470b",
    urls = [
        "https://github.com/kubernetes/repo-infra/archive/v0.1.4.tar.gz",
    ],
)

load("@io_k8s_repo_infra//:load.bzl", repo_infra_repositories = "repositories")

repo_infra_repositories()

load("@io_k8s_repo_infra//:repos.bzl", repo_infra_configure = "configure", repo_infra_go_repositories = "go_repositories")

# IMPORTANT: Only one go version may be specified at a time
#            'go_version':          used to specify a published upstream go version
#            'override_go_version': used to specify an alternate go version provided
#                                   by kubernetes/repo-infra
repo_infra_configure(
    go_version = "1.15.8",
    minimum_bazel_version = "2.2.0",
)

repo_infra_go_repositories()

# begin setup rules_docker
http_archive(
    name = "io_bazel_rules_docker",
    sha256 = "4521794f0fba2e20f3bf15846ab5e01d5332e587e9ce81629c7f96c793bb7036",
    strip_prefix = "rules_docker-0.14.4",
    urls = mirror("https://github.com/bazelbuild/rules_docker/releases/download/v0.14.4/rules_docker-v0.14.4.tar.gz"),
)

load(
    "@io_bazel_rules_docker//repositories:repositories.bzl",
    container_repositories = "repositories",
)

container_repositories()

load("@io_bazel_rules_docker//repositories:deps.bzl", container_deps = "deps")

container_deps()

load("@io_bazel_rules_docker//repositories:pip_repositories.bzl", "pip_deps")

pip_deps()

load(
    "@io_bazel_rules_docker//container:container.bzl",
    "container_pull",
)

# end setup rules_docker

container_pull(
    name = "distroless_base",
    digest = "sha256:7fa7445dfbebae4f4b7ab0e6ef99276e96075ae42584af6286ba080750d6dfe5",
    registry = "gcr.io",
    repository = "distroless/base",
    tag = "latest",  # ignored when digest provided, but kept here for documentation.
)

load("//build:workspace.bzl", "release_dependencies")

release_dependencies()

load("//build:workspace_mirror.bzl", "export_urls")

export_urls("workspace_urls")

load("@bazel_skylib//:workspace.bzl", "bazel_skylib_workspace")

bazel_skylib_workspace()
