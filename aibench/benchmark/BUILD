package(
    default_visibility = ["//visibility:public"],
)

licenses(["notice"])  # Apache 2.0

load(
    "//aibench:aibench.bzl",
    "if_android",
    "if_not_android",
    "if_android_arm64",
    "if_android_armv7",
    "if_mace_enabled",
    "if_mace_dsp_enabled",
    "if_ncnn_enabled",
    "if_snpe_enabled",
    "if_tflite_enabled",
)

cc_library(
    name = "benchmark",
    testonly = 1,
    srcs = [
        "benchmark.cc",
        "imagenet/imagenet_preprocessor.cc",
        "imagenet/imagenet_postprocessor.cc",
    ],
    hdrs = [
        "benchmark.h",
        "imagenet/imagenet_preprocessor.h",
        "imagenet/imagenet_postprocessor.h",
    ],
    deps = [
        "//aibench/executors:base_executor",
        "@mace//mace/utils",
    ] + if_not_android([
        "@opencv//:opencv_x86_64",
    ]) + if_android_armv7([
        "@opencv//:opencv_armeabi-v7a",
    ])+ if_android_arm64([
        "@opencv//:opencv_arm64-v8a",
    ]),
    alwayslink = 1,
)

cc_test(
    name = "model_benchmark",
    testonly = 1,
    srcs = ["benchmark_main.cc"],
    copts = if_mace_enabled([
        "-DAIBENCH_ENABLE_MACE",
    ]) + if_mace_dsp_enabled([
        "-DAIBENCH_ENABLE_MACE_DSP",
    ]) + if_ncnn_enabled([
        "-DAIBENCH_ENABLE_NCNN",
    ]) + if_snpe_enabled([
        "-DAIBENCH_ENABLE_SNPE",
    ]) + if_tflite_enabled([
        "-DAIBENCH_ENABLE_TFLITE",
    ]),
    linkopts = [
        "-fopenmp",
        "-ldl",
    ] + if_android([
        "-pie",
        "-lm",
    ]),
    linkstatic = 1,
    deps = [
        "benchmark",
        "//external:gflags_nothreads",
    ] + if_mace_enabled([
        "//aibench/executors:mace_executor",
    ]) + if_ncnn_enabled([
        "//aibench/executors:ncnn_executor",
    ]) + if_snpe_enabled([
        "//aibench/executors:snpe_executor",
    ]) + if_tflite_enabled([
        "//aibench/executors:tflite_executor",
    ]),
)
