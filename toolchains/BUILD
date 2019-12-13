load(
    "@rules_scala_annex//rules:scala.bzl",
    "configure_bootstrap_scala",
    "configure_zinc_scala",
    "scala_library",
)

compiler_classpath_2_11_12 = [
    "@scala_2_11//:org_scala_lang_scala_compiler",
    "@scala_2_11//:org_scala_lang_scala_library",
    "@scala_2_11//:org_scala_lang_scala_reflect",
]

runtime_classpath_2_11_12 = [
    "@scala_2_11//:org_scala_lang_scala_library",
]

configure_bootstrap_scala(
    name = "bootstrap_2_11_12",
    compiler_classpath = compiler_classpath_2_11_12,
    runtime_classpath = runtime_classpath_2_11_12,
    version = "2.11.12",
    visibility = ["//visibility:public"],
)

scala_library(
    name = "compiler_bridge_2_11_12",
    srcs = [
        "@compiler_bridge_2_11//:src",
    ],
    scala = ":bootstrap_2_11_12",
    visibility = ["//visibility:public"],
    deps = compiler_classpath_2_11_12 + [
        "@scala_2_11//:org_scala_sbt_compiler_interface",
        "@scala_2_11//:org_scala_sbt_util_interface",
    ],
)

configure_zinc_scala(
    name = "zinc_2_11_12",
    compiler_bridge = ":compiler_bridge_2_11_12",
    compiler_classpath = compiler_classpath_2_11_12,
    # turning this to warning for now due to docker rules not having deps_used_whitelist
    #deps_used = "warn",
    global_plugins = [
        "@scala_2_11//:org_wartremover_wartremover_2_11_12",
    ],
    global_scalacopts = [
        "-Ypartial-unification",
        # see https://github.com/digital-asset/daml/blob/ea02814b343e4754c70a8718cb14657d6c51915f/bazel_tools/scala.bzl#L66 for example warts
        "-P:wartremover:traverser:org.wartremover.warts.Product",
        "-Xplugin-require:wartremover",
        # this ignores generated code like that created by play-routes compilation
        "-P:wartremover:excluded:bazel-out",
        "-encoding",
        "UTF-8",
        "-deprecation",  # warning and location for usages of deprecated APIs
        "-feature",  # warning and location for usages of features that should be imported explicitly
        "-unchecked",  # additional warnings where generated code depends on assumptions
        "-Xlint",  # recommended additional warnings
        "-Ywarn-adapted-args",  # Warn if an argument list is modified to match the receiver
        "-Ywarn-inaccessible",
        "-Ywarn-dead-code",
        "-Ywarn-numeric-widen",
    ],
    runtime_classpath = runtime_classpath_2_11_12,
    version = "2.11.12",
    visibility = ["//visibility:public"],
)
