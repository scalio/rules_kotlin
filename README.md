![Bazel Kotlin at Scalio](scalio-bazel-kotlin.svg)

[![Build status](https://badge.buildkite.com/a8860e94a7378491ce8f50480e3605b49eb2558cfa851bbf9b.svg)](https://buildkite.com/bazel/kotlin-postsubmit)

[Skydoc documentation](https://bazelbuild.github.io/rules_kotlin)

# Announcements
* <b>July 4, 2019.</b> Kotlin 1.3 support with multiply build bugfixes fixes.
* <b>April 1, 2019.</b> [Roadmap](https://github.com/bazelbuild/rules_kotlin/blob/master/ROADMAP.md) for rules_kotlin published.
* <b>February 20, 2019.</b> [Future directions](https://github.com/bazelbuild/rules_kotlin/issues/174) of rules_kotlin.
* <b>August 14, 2018.</b> Js support. No documentation yet but see the nested example workspace `examples/node`.
* <b>August 14, 2018.</b> Android support. No documentation but it's a simple integration. see 
  `kotlin/internal/jvm/android.bzl`.
* <b>Jun 29, 2018.</b> The commits from this date forward are compatible with bazel `>=0.14`. JDK9 host issues were 
  fixed as well some other deprecations. I recommend skipping `0.15.0` if you   are on a Mac. 
* <b>May 25, 2018.</b> Test "friend" support. A single friend dep can be provided to `kt_jvm_test` which allows the test
  to access internal members of the module under test.
* <b>February 15, 2018.</b> Toolchains for the JVM rules. Currently this allow tweaking: 
    * The JVM target (bytecode level).
    * API and Language levels.
    * Coroutines, enabled by default. 
* <b>February 9, 2018.</b> Annotation processing.
* <b>February 5, 2018. JVM rule name change:</b> the prefix has changed from `kotlin_` to `kt_jvm_`.

# Overview 

These rules were forked from [bazelbuild/rules_kotlin](https://github.com/bazelbuild/rules_kotlin) and initially forked from [pubref/rules_kotlin](http://github.com/pubref/rules_kotlin).
Key changes:

* Replace the macros with three basic rules. `kt_jvm_binary`, `kt_jvm_library` and `kt_jvm_test`.
* Use a single dep attribute instead of `java_dep` and `dep`.
* Add support for the following standard java rules attributes:
  * `data`
  * `resource_jars`
  * `runtime_deps`
  * `resources`
  * `resources_strip_prefix`
  * `exports`
* Persistent worker support.
* Mixed-Mode compilation (compile Java and Kotlin in one pass).

# Quick Guide
This section just contains a quick overview. Consult the generated 
[documentation](https://bazelbuild.github.io/rules_kotlin). Note: Skydoc documentation is no longer being generated. 
Comprehensive documentation will have to wait till the new documentation generation tool is ready. A contribution to 
port the documentation to the RST format like `rules_go` has would be very welcome !


## WORKSPACE
In the project's `WORKSPACE`, declare the external repository and initialize the toolchains, like
this:

```build
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

rules_kotlin_version = "VERSION_0.1"

http_archive(
    name = "io_bazel_rules_kotlin",
    urls = ["https://github.com/scalio/rules_kotlin/archive/%s.zip" % rules_kotlin_version],
    type = "zip",
    strip_prefix = "rules_kotlin-%s" % rules_kotlin_version,
    sha256 = "7a463fa65b49fb686ad0e24eca7a4a8f6dc884bb7392ad8dcbfe44e0616091e5"
)

load("@io_bazel_rules_kotlin//kotlin:kotlin.bzl", "kotlin_repositories", "kt_register_toolchains")
kotlin_repositories()
kt_register_toolchains()
```

## BUILD files

In your project's `BUILD` files, load the kotlin rules and use them like so:

```
load("@io_bazel_rules_kotlin//kotlin:kotlin.bzl", "kt_jvm_library")

kt_jvm_library(
    name = "package_name",
    srcs = glob(["*.kt"]),
    deps = [
        "//path/to/dependency",
    ],
)

load("@io_bazel_rules_kotlin//kotlin:kotlin.bzl", "kt_android_library")

kt_android_library(
    name = "package_name",
    srcs = glob(["*.kt"]),
    deps = [
        "//path/to/dependency",
    ]
)

```

# License

This project is licensed under the [Apache 2.0 license](LICENSE), as are all contributions

# Contributing

Pull Requests are welcome.
