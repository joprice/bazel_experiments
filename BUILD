java_runtime(
    name = "jdk",
    srcs = select({
        "@bazel_tools//src/conditions:linux_x86_64": ["@jdk8-linux//:jdk"],
        "@bazel_tools//src/conditions:darwin_x86_64": ["@jdk8-osx//:jdk"],
        "@bazel_tools//src/conditions:darwin": ["@jdk8-osx//:jdk"],
    }),
    java = select({
        "@bazel_tools//src/conditions:linux_x86_64": "@jdk8-linux//:java",
        "@bazel_tools//src/conditions:darwin_x86_64": "@jdk8-osx//:java",
        "@bazel_tools//src/conditions:darwin": "@jdk8-osx//:java",
    }),
    visibility = ["//visibility:public"],
)

load(
    "@rules_scala_annex//rules:scala.bzl",
    "scala_binary",
    "scala_library",
    "scala_repl",
    "scala_test",
)
load("@io_bazel_rules_docker//scala:image.bzl", "scala_image")
load("@io_bazel_rules_docker//container:container.bzl", "container_push")

scala_binary(
    name = "service",
    main_class = "com.joprice.Main",
    srcs = ["Main.scala"]
)

scala_image(
    name = "image",
    main_class = "com.joprice.Main",
    runtime_deps = ["service"],
    deps = ["service"],
)
