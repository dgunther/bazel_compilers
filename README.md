[![Actions Status](https://github.com/curtismuntz/bazel_compilers/workflows/ci/badge.svg)](https://github.com/curtismuntz/bazel_compilers/actions)

# bazel_compilers

This repo hosts the bazel compiler defintions for my many cpp projects. I'm not a compiler expert, so use with caution. I'm trying to maintain a Raspberry Pi / Nvidia Jetson suite of compiler definitions in one repository, effectively mirroring the bazel crosstool [here](https://github.com/bazelbuild/bazel/tree/master/src/test/shell/bazel/testdata/bazel_toolchain_test_data)

Requires bazel version >= 0.25.1.

Supported compilers:

* armv7hf: processor architecture used in the TI AM335x family of embedded products.

# usage

In your `WORKSPACE` file:

```
http_archive(
    name = "supervisor_bazel_compilers",
    urls = [
      "https://github.com/dgunther/bazel_compilers/archive/<commit hash>.tar.gz",
    ],
    strip_prefix = "bazel_compilers-< commit hash >",
)

load("@murtis_bazel_compilers//compilers:dependencies.bzl", "cross_compiler_dependencies")

cross_compiler_dependencies()
```

In your `.bazelrc` file:

```
build --compiler=compiler

build:armv7hf --crosstool_top=@supervisor_bazel_compilers//compilers/arm_compiler:toolchain
build:armv7hf --host_crosstool_top=@bazel_tools//tools/cpp:toolchain
build:armv7hf --cpu=armeabi-v7a --compiler=gcc
build:armv7hf --spawn_strategy=standalone
```


Build with:

```
# locally executable:
bazel build //example

# BeagleBone:
bazel build --config=armv7hf //example


```
