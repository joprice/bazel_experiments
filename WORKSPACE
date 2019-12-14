load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

rules_scala_annex_version = "67ff718d4453e6455debee6289dd7123e26711cb"

rules_scala_annex_sha256 = "84775c66bc6dd9d87dceb625d432c3193f95c355399d89b100e2c8a812d5f26c"

http_archive(
    name = "rules_scala_annex",
    sha256 = rules_scala_annex_sha256,
    strip_prefix = "rules_scala-{}".format(rules_scala_annex_version),
    url = "https://github.com/higherkindness/rules_scala/archive/{}.zip".format(rules_scala_annex_version),
)

rules_jvm_external_tag = "140a3a24d38913b17051c738218ddc659ce93026"

http_archive(
    name = "rules_jvm_external",
    sha256 = "5893c5b1c0c64a47261cd02d2ace6be27abbced5cbb41e4eb750bef9d400b7ae",
    strip_prefix = "rules_jvm_external-{}".format(rules_jvm_external_tag),
    url = "https://github.com/bazelbuild/rules_jvm_external/archive/{}.zip".format(rules_jvm_external_tag),
)

# use this to test changes locally
#local_repository(
#    name = "rules_jvm_external",
#    path = "../../bazelbuild/rules_jvm_external/"
#)

load("@rules_scala_annex//rules/scala:workspace.bzl", "scala_register_toolchains", "scala_repositories")

scala_repositories()

# Load bazel skylib and google protobuf
bazel_skylib_tag = "1.0.2"

bazel_skylib_sha256 = "97e70364e9249702246c0e9444bccdc4b847bed1eb03c5a3ece4f83dfe6abc44"

http_archive(
    name = "bazel_skylib",
    sha256 = bazel_skylib_sha256,
    urls = [
        "https://mirror.bazel.build/github.com/bazelbuild/bazel-skylib/releases/download/{tag}/bazel-skylib-{tag}.tar.gz".format(tag = bazel_skylib_tag),
        "https://github.com/bazelbuild/bazel-skylib/releases/download/{tag}/bazel-skylib-{tag}.tar.gz".format(tag = bazel_skylib_tag),
    ],
)

protobuf_tag = "3.10.1"

protobuf_sha256 = "678d91d8a939a1ef9cb268e1f20c14cd55e40361dc397bb5881e4e1e532679b1"

http_archive(
    name = "com_google_protobuf",
    sha256 = protobuf_sha256,
    strip_prefix = "protobuf-{}".format(protobuf_tag),
    type = "zip",
    url = "https://github.com/protocolbuffers/protobuf/archive/v{}.zip".format(protobuf_tag),
)

load("@com_google_protobuf//:protobuf_deps.bzl", "protobuf_deps")

protobuf_deps()

bind(
    name = "default_scala",
    actual = "//toolchains:zinc_2_11_12",
)

load("@rules_scala_annex//rules:rules_scala.bzl", "emulate_rules_scala")

emulate_rules_scala(
    scala = "//toolchains:zinc_2_11_12",
    scalatest = "@scala_2_11//:org_scalatest_scalatest_2_11",
)

load("@annex//:defs.bzl", annex_pinned_maven_install = "pinned_maven_install")

annex_pinned_maven_install()

scala_register_toolchains()

load("@rules_scala_annex//rules/scalafmt:workspace.bzl", "scalafmt_default_config", "scalafmt_repositories")

scalafmt_repositories()

load("@annex_scalafmt//:defs.bzl", annex_scalafmt_pinned_maven_install = "pinned_maven_install")

annex_scalafmt_pinned_maven_install()

scalafmt_default_config()

load("@rules_scala_annex//rules/scala_proto:workspace.bzl", "scala_proto_register_toolchains", "scala_proto_repositories")

scala_proto_repositories()

load("@annex_proto//:defs.bzl", annex_proto_pinned_maven_install = "pinned_maven_install")

annex_proto_pinned_maven_install()

scala_proto_register_toolchains()

http_archive(
    name = "io_bazel_rules_docker",
    sha256 = "14ac30773fdb393ddec90e158c9ec7ebb3f8a4fd533ec2abbfd8789ad81a284b",
    strip_prefix = "rules_docker-0.12.1",
    urls = ["https://github.com/bazelbuild/rules_docker/releases/download/v0.12.1/rules_docker-v0.12.1.tar.gz"],
)

load(
    "@io_bazel_rules_docker//repositories:repositories.bzl",
    container_repositories = "repositories",
)

container_repositories()

load(
    "@io_bazel_rules_docker//scala:image.bzl",
    _scala_image_repos = "repositories",
)

_scala_image_repos()

load("@rules_jvm_external//:defs.bzl", "maven_install")

scala_version = "2.11.12"

maven_install(
    name = "scala_2_11",
    artifacts = [
        "org.scala-lang:scala-library:" + scala_version,
        "org.scala-lang:scala-compiler:" + scala_version,
        "org.scala-lang:scala-reflect:" + scala_version,
        "org.scala-sbt:zinc_2.11:1.2.1",
        "org.scala-sbt:compiler-interface:1.2.1",
        "org.scala-sbt:util-interface:1.2.0",
        "org.wartremover:wartremover_2.11.12:2.4.3",
        "com.typesafe.play:play_2.11:2.5.19",
        "org.scalatestplus.play:scalatestplus-play_2.11:2.0.1",
    ],
    repositories = [
      "https://repo1.maven.org/maven2",
    ],
    version_conflict_policy = "pinned",
    fetch_sources = True,
    use_unsafe_shared_cache = True,
    maven_install_json = "//:scala_2_11_install.json",
)

load("@scala_2_11//:defs.bzl", "pinned_maven_install")
pinned_maven_install()

# Java 8 is needed for Scala 2.11; this is needed to enable that
jdk_build_file_content = """
filegroup(
    name = "jdk",
    srcs = glob(["**/*"]),
    visibility = ["//visibility:public"],
)
filegroup(
    name = "java",
    srcs = ["bin/java"],
    visibility = ["//visibility:public"],
)
"""

http_archive(
    name = "jdk8-linux",
    build_file_content = jdk_build_file_content,
    sha256 = "dd28d6d2cde2b931caf94ac2422a2ad082ea62f0beee3bf7057317c53093de93",
    strip_prefix = "jdk8u212-b03",
    url = "https://github.com/AdoptOpenJDK/openjdk8-binaries/releases/download/jdk8u212-b03/OpenJDK8U-jdk_x64_linux_hotspot_8u212b03.tar.gz",
)

http_archive(
    name = "jdk8-osx",
    build_file_content = jdk_build_file_content,
    sha256 = "3d80857e1bb44bf4abe6d70ba3bb2aae412794d335abe46b26eb904ab6226fe0",
    strip_prefix = "jdk8u212-b03/Contents/Home",
    url = "https://github.com/AdoptOpenJDK/openjdk8-binaries/releases/download/jdk8u212-b03/OpenJDK8U-jdk_x64_mac_hotspot_8u212b03.tar.gz",
)
