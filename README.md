# zig-bootstrap

The purpose of this project is to start with minimum system dependencies and
end with a fully operational Zig compiler for any target.

## Version Information

This repository copies sources from upstream. Patches listed below. Use git
to find and inspect the patch diffs.

 * LLVM, LLD, Clang 23.1.4
 * zlib 1.3.1
 * zstd 1.5.2
 * zig 0.18.0-dev.1192+5149e39a7

For other versions, check the git tags of this repository.

### Patches

 * all: Deleted unused files.
 * LLVM: Support .lib extension for static zstd.
 * LLVM: Don't pass -static when building executables.
 * LLVM: OpenBSD `llvm-config` logic
 * LLVM: Handle division by zero trap on MIPS1
 * Clang: Ignore the examples directory
 * Clang: Disable building of libclang-cpp.so.
 * Clang: remove `nvptx-arch` and `amdgpu-arch` symlinks
 * Clang: remove broken scan-build manpage install logic
 * LLD: Added additional include directory to Zig's libunwind.
 * LLD: Respect `LLD_BUILD_TOOLS=OFF`
 * LLD: Skip building docs
 * LLD: OpenBSD `findMajMinShlib()` logic
 * zlib: Delete the ability to build a shared library.

## Host System Dependencies

 * C++ compiler capable of building LLVM, Clang, and LLD from source (GCC 12+
   or Clang)
     * On some systems, static libstdc++/libc++ may need to be installed
 * CMake 3.20 or later
 * make, ninja, or any other build system supported by CMake
 * POSIX system (bash, mkdir, cd)
 * Python 3

## Build Instructions

```
./build <arch>-<os>-<abi> <mcpu>
```

All parameters are required:

 * `<arch>-<os>-<abi>`: Replace with one of the Supported Targets below, or use
   `native` for the `<arch>` value (e.g. `native-linux-gnu`) to use the native
   architecture.
 * `<mcpu>`: Replace with a `-mcpu` parameter of Zig. `baseline` is recommended
   and means it will target a generic CPU for the target. `native` means it
   will target the native CPU. See the Zig documentation for more details.

Please be aware of the following two CMake environment variables that can
significantly affect how long it takes to build:

 * `CMAKE_GENERATOR` can be used to select a different generator instead of the
   default. For example, `CMAKE_GENERATOR=Ninja`.
 * `CMAKE_BUILD_PARALLEL_LEVEL` can be used to introduce parallelism to build
   systems (such as make) which do not default to parallel builds. This option
   is irrelevant when using Ninja.

When it succeeds, output can be found in `out/zig-<target>-<cpu>/`.

## Windows Build Instructions

Bootstrapping on Windows with MSVC is also possible via `build.bat`, which
takes the same arguments as `build` above.

This script requires that the "C++ CMake tools for Windows" component be
installed via the Visual Studio installer.

The script must be run within the `Developer Command Prompt for VS 2019` shell:

```
build.bat <arch>-<os>-<abi> <mcpu>
```

To build for x86 Windows, run the script within the `x86 Native Tools Command Prompt for VS 2019`.

### Supported Targets

Note: Generally, for Linux targets, we prefer the musl libc builds over the
glibc builds here, because musl builds end up producing a static binary, which
is more portable across Linux distributions.

#### FreeBSD

| Target                     | Status |
|----------------------------|--------|
| `aarch64-freebsd-none`     | OK     |
| `arm-freebsd-none`         | OK     |
| `powerpc64-freebsd-none`   | OK     |
| `powerpc64le-freebsd-none` | OK     |
| `riscv64-freebsd-none`     | OK     |
| `x86_64-freebsd-none`      | OK     |

#### Linux

| Target                       | Status |
|------------------------------|--------|
| `aarch64-linux-gnu`          | OK     |
| `aarch64-linux-musl`         | OK     |
| `aarch64be-linux-gnu`        | OK     |
| `aarch64be-linux-musl`       | OK     |
| `arm-linux-gnu`              | OK     |
| `arm-linux-gnusf`            | OK     |
| `arm-linux-musl`             | OK     |
| `arm-linux-muslsf`           | OK     |
| `armeb-linux-gnu`            | OK     |
| `armeb-linux-gnusf`          | OK     |
| `armeb-linux-musl`           | OK     |
| `armeb-linux-muslsf`         | OK     |
| `hexagon-linux-musl`         | [#269](https://codeberg.org/ziglang/zig-bootstrap/issues/269) |
| `loongarch64-linux-gnu`      | OK     |
| `loongarch64-linux-gnusf`    | OK     |
| `loongarch64-linux-musl`     | OK     |
| `loongarch64-linux-muslf32`  | [#267](https://codeberg.org/ziglang/zig-bootstrap/issues/267) |
| `loongarch64-linux-muslsf`   | OK     |
| `mips-linux-gnu`             | OK     |
| `mips-linux-gnusf`           | OK     |
| `mips-linux-musl`            | OK     |
| `mips-linux-muslsf`          | OK     |
| `mipsel-linux-gnu`           | OK     |
| `mipsel-linux-gnusf`         | OK     |
| `mipsel-linux-musl`          | OK     |
| `mipsel-linux-muslsf`        | OK     |
| `mipsf32-linux-gnu`          | [#268](https://codeberg.org/ziglang/zig-bootstrap/issues/268) |
| `mipsf32-linux-musl`         | [#268](https://codeberg.org/ziglang/zig-bootstrap/issues/268) |
| `mipsf32el-linux-gnu`        | [#268](https://codeberg.org/ziglang/zig-bootstrap/issues/268) |
| `mipsf32el-linux-musl`       | [#268](https://codeberg.org/ziglang/zig-bootstrap/issues/268) |
| `mipsf64-linux-gnu`          | OK     |
| `mipsf64-linux-musl`         | OK     |
| `mipsf64el-linux-gnu`        | OK     |
| `mipsf64el-linux-musl`       | OK     |
| `mipsf64n8-linux-gnu`        | OK     |
| `mipsf64n8-linux-musl`       | OK     |
| `mipsf64n8el-linux-gnu`      | OK     |
| `mipsf64n8el-linux-musl`     | OK     |
| `mipsn8-linux-gnu`           | OK     |
| `mipsn8-linux-gnusf`         | OK     |
| `mipsn8-linux-musl`          | OK     |
| `mipsn8-linux-muslsf`        | OK     |
| `mipsn8el-linux-gnu`         | OK     |
| `mipsn8el-linux-gnusf`       | OK     |
| `mipsn8el-linux-musl`        | OK     |
| `mipsn8el-linux-muslsf`      | OK     |
| `mipsr6-linux-gnu`           | [#270](https://codeberg.org/ziglang/zig-bootstrap/issues/270) |
| `mipsr6-linux-gnusf`         | [#270](https://codeberg.org/ziglang/zig-bootstrap/issues/270) |
| `mipsr6-linux-musl`          | [#270](https://codeberg.org/ziglang/zig-bootstrap/issues/270) |
| `mipsr6-linux-muslsf`        | [#270](https://codeberg.org/ziglang/zig-bootstrap/issues/270) |
| `mipsr6el-linux-gnu`         | [#270](https://codeberg.org/ziglang/zig-bootstrap/issues/270) |
| `mipsr6el-linux-gnusf`       | [#270](https://codeberg.org/ziglang/zig-bootstrap/issues/270) |
| `mipsr6el-linux-musl`        | [#270](https://codeberg.org/ziglang/zig-bootstrap/issues/270) |
| `mipsr6el-linux-muslsf`      | [#270](https://codeberg.org/ziglang/zig-bootstrap/issues/270) |
| `mips64-linux-gnu`           | OK     |
| `mips64-linux-gnusf`         | OK     |
| `mips64-linux-gnun32`        | OK     |
| `mips64-linux-gnun32sf`      | OK     |
| `mips64-linux-musl`          | OK     |
| `mips64-linux-muslsf`        | OK     |
| `mips64-linux-musln32`       | OK     |
| `mips64-linux-musln32sf`     | OK     |
| `mips64el-linux-gnu`         | OK     |
| `mips64el-linux-gnusf`       | OK     |
| `mips64el-linux-gnun32`      | OK     |
| `mips64el-linux-gnun32sf`    | OK     |
| `mips64el-linux-musl`        | OK     |
| `mips64el-linux-muslsf`      | OK     |
| `mips64el-linux-musln32`     | OK     |
| `mips64el-linux-musln32sf`   | OK     |
| `mips64n8-linux-gnu`         | OK     |
| `mips64n8-linux-gnusf`       | OK     |
| `mips64n8-linux-gnun32`      | OK     |
| `mips64n8-linux-gnun32sf`    | OK     |
| `mips64n8-linux-musl`        | OK     |
| `mips64n8-linux-muslsf`      | OK     |
| `mips64n8-linux-musln32`     | OK     |
| `mips64n8-linux-musln32sf`   | OK     |
| `mips64n8el-linux-gnu`       | OK     |
| `mips64n8el-linux-gnusf`     | OK     |
| `mips64n8el-linux-gnun32`    | OK     |
| `mips64n8el-linux-gnun32sf`  | OK     |
| `mips64n8el-linux-musl`      | OK     |
| `mips64n8el-linux-muslsf`    | OK     |
| `mips64n8el-linux-musln32`   | OK     |
| `mips64n8el-linux-musln32sf` | OK     |
| `mips64r6-linux-gnu`         | [#270](https://codeberg.org/ziglang/zig-bootstrap/issues/270) |
| `mips64r6-linux-gnusf`       | [#270](https://codeberg.org/ziglang/zig-bootstrap/issues/270) |
| `mips64r6-linux-gnun32`      | [#270](https://codeberg.org/ziglang/zig-bootstrap/issues/270) |
| `mips64r6-linux-gnun32sf`    | [#270](https://codeberg.org/ziglang/zig-bootstrap/issues/270) |
| `mips64r6-linux-musl`        | [#270](https://codeberg.org/ziglang/zig-bootstrap/issues/270) |
| `mips64r6-linux-muslsf`      | [#270](https://codeberg.org/ziglang/zig-bootstrap/issues/270) |
| `mips64r6-linux-musln32`     | [#270](https://codeberg.org/ziglang/zig-bootstrap/issues/270) |
| `mips64r6-linux-musln32sf`   | [#270](https://codeberg.org/ziglang/zig-bootstrap/issues/270) |
| `mips64r6el-linux-gnu`       | [#270](https://codeberg.org/ziglang/zig-bootstrap/issues/270) |
| `mips64r6el-linux-gnusf`     | [#270](https://codeberg.org/ziglang/zig-bootstrap/issues/270) |
| `mips64r6el-linux-gnun32`    | [#270](https://codeberg.org/ziglang/zig-bootstrap/issues/270) |
| `mips64r6el-linux-gnun32sf`  | [#270](https://codeberg.org/ziglang/zig-bootstrap/issues/270) |
| `mips64r6el-linux-musl`      | [#270](https://codeberg.org/ziglang/zig-bootstrap/issues/270) |
| `mips64r6el-linux-muslsf`    | [#270](https://codeberg.org/ziglang/zig-bootstrap/issues/270) |
| `mips64r6el-linux-musln32`   | [#270](https://codeberg.org/ziglang/zig-bootstrap/issues/270) |
| `mips64r6el-linux-musln32sf` | [#270](https://codeberg.org/ziglang/zig-bootstrap/issues/270) |
| `powerpc-linux-musl`         | OK     |
| `powerpc-linux-muslsf`       | OK     |
| `powerpc64-linux-musl`       | OK     |
| `powerpc64le-linux-gnu`      | OK     |
| `powerpc64le-linux-musl`     | OK     |
| `riscv32-linux-gnu`          | OK     |
| `riscv32-linux-gnusf`        | OK     |
| `riscv32-linux-musl`         | OK     |
| `riscv32-linux-muslf32`      | OK     |
| `riscv32-linux-muslsf`       | OK     |
| `riscv64-linux-gnu`          | OK     |
| `riscv64-linux-gnusf`        | OK     |
| `riscv64-linux-musl`         | OK     |
| `riscv64-linux-muslf32`      | OK     |
| `riscv64-linux-muslsf`       | OK     |
| `s390x-linux-gnu`            | OK     |
| `s390x-linux-musl`           | OK     |
| `sparc-linux-gnu`            | [#117](https://codeberg.org/ziglang/zig-bootstrap/issues/117) |
| `sparc64-linux-gnu`          | OK     |
| `thumb-linux-musl`           | OK     |
| `thumb-linux-muslsf`         | OK     |
| `thumbeb-linux-musl`         | OK     |
| `thumbeb-linux-muslsf`       | OK     |
| `x86-linux-gnu`              | OK     |
| `x86-linux-musl`             | OK     |
| `x86_64-linux-gnu`           | OK     |
| `x86_64-linux-gnux32`        | OK     |
| `x86_64-linux-musl`          | OK     |
| `x86_64-linux-muslx32`       | OK     |

#### macOS

| Target               | Status |
|----------------------|--------|
| `aarch64-macos-none` | OK     |
| `x86_64-macos-none`  | OK     |

#### NetBSD

| Target                   | Status |
|--------------------------|--------|
| `aarch64-netbsd-none`    | OK     |
| `aarch64be-netbsd-none`  | OK     |
| `arm-netbsd-none`        | OK     |
| `arm-netbsd-sf`          | OK     |
| `armeb-netbsd-none`      | OK     |
| `armeb-netbsd-sf`        | OK     |
| `mips-netbsd-none`       | OK     |
| `mips-netbsd-sf`         | OK     |
| `mipsel-netbsd-none`     | OK     |
| `mipsel-netbsd-sf`       | OK     |
| `mipsf32-netbsd-none`    | [#266](https://codeberg.org/ziglang/zig-bootstrap/issues/266) |
| `mipsf32el-netbsd-none`  | [#266](https://codeberg.org/ziglang/zig-bootstrap/issues/266) |
| `mips64-netbsd-sf`       | OK     |
| `mips64-netbsd-n32sf`    | OK     |
| `mips64el-netbsd-sf`     | OK     |
| `mips64el-netbsd-n32sf`  | OK     |
| `powerpc-netbsd-none`    | OK     |
| `riscv32-netbsd-none`    | OK     |
| `riscv64-netbsd-none`    | OK     |
| `sparc-netbsd-none`      | [#230](https://codeberg.org/ziglang/zig-bootstrap/issues/230) |
| `sparc64-netbsd-none`    | OK     |
| `x86-netbsd-none`        | OK     |
| `x86_64-netbsd-none`     | OK     |

#### OpenBSD

| Target                   | Status |
|--------------------------|--------|
| `aarch64-openbsd-none`   | OK     |
| `arm-openbsd-sf`         | OK     |
| `mips64-openbsd-none`    | OK     |
| `powerpc-openbsd-none`   | OK     |
| `powerpc64-openbsd-none` | OK     |
| `riscv64-openbsd-none`   | OK     |
| `sparc64-openbsd-none`   | OK     |
| `x86-openbsd-none`       | OK     |
| `x86_64-openbsd-none`    | OK     |

#### Windows

| Target                | Status |
|-----------------------|--------|
| `aarch64-windows-gnu` | OK     |
| `thumb-windows-gnu`   | OK     |
| `x86-windows-gnu`     | OK     |
| `x86_64-windows-gnu`  | OK     |
