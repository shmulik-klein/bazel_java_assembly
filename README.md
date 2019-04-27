# Bazel java assembly tools

The tool provide a way to create a java jar with all it's dependencies, but it will exclude unused deps from the jar to make it thin.

A dep jar will be in the final jar if:
1. It's used by the lib (the tools uses jdeps created by bazel to know that).
2. It's an explicit dep of the lib.

## Usage

Add to your `WORKSPACE` file:
```
load("@bazel_tools//tools/build_defs/repo:git.bzl", "git_repository")
git_repository(
    name = "java_assembly",
    remote = "https://github.com/zacharya19/bazel_java_assembly.git",
    commit = "811d594341b3f240bf44add118deca2c5ef5bf77"
)
```

Add to your BUILD file:
```
load("@java_assembly//:assembly.bzl", "java_assembly_jar")
java_assembly_jar(
    name = "jar",
    srcs = glob(["src/main/**/*.java"]),
    deps = deps,
)
```

Run `bazel build :jar_assembly`.

The `java_assembly_jar` rule will create a `java_library` named jar with all the args, and an assembly jar named `jar_assembly`, so you can build both.

## TODO
1. Extend the tool to build java WAR files.
2. Write tests.
3. Add an examples.