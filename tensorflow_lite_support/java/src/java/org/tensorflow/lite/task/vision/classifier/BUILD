load("@build_bazel_rules_android//android:rules.bzl", "android_library")
load("@org_tensorflow//tensorflow/lite/core/shims:cc_library_with_tflite.bzl", "android_library_with_tflite")
load("@org_tensorflow//tensorflow/lite/java:aar_with_jni.bzl", "aar_with_jni")

package(
    default_visibility = ["//tensorflow_lite_support:users"],
    licenses = ["notice"],  # Apache 2.0
)

exports_files([
    "AndroidManifest.xml",
])

filegroup(
    name = "image_classifier_src",
    srcs = glob(
        ["**/*.java"],
    ),
)

# Default target that uses BuiltInOpResolver, registers all built-in OPs.
android_library_with_tflite(
    name = "image_classifier",
    tflite_exports = [
        "//tensorflow_lite_support/java/src/native/task/vision/classifier:image_classifier_native",
    ],
    exports = [
        ":image_classifier_java",
    ],
)

# Java-only target, needs to be used together with a native target similar to
# //third_party/tensorflow_lite_support/java/src/native/task/vision/classifier:image_classifier_native.
# Use this target when you want to provide a MutableOpResolver with customized
# OPs and/or a subset of BuiltInOps to reduce binary size.
android_library(
    name = "image_classifier_java",
    srcs = [
        ":image_classifier_src",
        "//tensorflow_lite_support/java/src/java/org/tensorflow/lite/task/vision/core:base_vision_api_src",
    ],
    javacopts = ["-source 7 -target 7"],
    manifest = "AndroidManifest.xml",
    deps = [
        "//tensorflow_lite_support/java:tensorflowlite_support_java",
        "//tensorflow_lite_support/java/src/java/org/tensorflow/lite/task/core:base_task_api",
        "@com_google_auto_value",
        "@org_tensorflow//tensorflow/lite/java:tensorflowlite_java",
    ],
)

# AAR target for OSS release.
#
# bazel build -c opt --config=monolithic --config=android_arm64 --fat_apk_cpu=x86,x86_64,arm64-v8a,armeabi-v7a \
#   tensorflow_lite_support/java/src/java/org/tensorflow/lite/task/vision/classifier:image-classifier
aar_with_jni(
    name = "image-classifier",
    android_library = ":image_classifier",
)
