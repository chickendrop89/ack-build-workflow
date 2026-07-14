# ack-build-workflow
A github workflow to build custom android common kernels with specified parameters

# How to use this?
[Fork this repo](https://github.com/ChicKernel/ack-build-workflow/fork), 
open the 'Actions' tab in your fork and run the only workflow there

These parameters are by default configured for [chickernel](https://github.com/ChicKernel/device_xiaomi_gemstones-kernel), 
however, they can be configured for any ACK if you know what you're doing.

Parameters:
- 🔴 Android-common-kernel branch:
    - ACK branch to use (e.g. `common-android13-5.15`)
    - Google provides a list of [possible ACK branches here](https://source.android.com/docs/setup/reference/bazel-support)
    - This is used for the build workspace (`prebuilts`, `external`, `build`)
  
    `default:` **common-android13-5.15**
- 🔴 Custom kernel repository:
    - Git URL leading to your custom kernel repository
    
    `default:` **[chickernel](https://github.com/ChicKernel/device_xiaomi_gemstones-kernel)**
- 🔴 Custom kernel repository branch:
    - Branch to use from your custom kernel repository

    `default:` **[android13-5.15-lts](https://github.com/ChicKernel/device_xiaomi_gemstones-kernel/tree/android13-5.15-lts)**
- 🔴 Use the latest clang prebuilts
    - If enabled, will override the ACK manifest to use upstream branch of `clang/host/linux-x86` prebuilts
    - On newer branches, select this strictly only if your `build.config.constants` uses the upstream clang toolchain.

    `default:` yes
- 🟡 Space-separated AnyKernel3 fork URL + (optional) branch:
    - Git URL leading to your AK3 fork, and optionally branch
    - Used in post-build to package the kernel image(s)

    `default:` **[chickernel AK3 fork](https://github.com/ChicKernel/AnyKernel3)**
- 🟡 Kernel image to package:
    - Decides what image type is packaged with AK3
    - This allows for using compressed `Image.<xxx>` or `Image.<xxx>-dtb`
  
    `default`: `Image`
- 🟢 Instruct tools to perform a Fast Build: 
    - If enabled, will perform a shorter build at the cost of skiping few optimizations.
    - Depending on the build system used, this can affect LTO configuration or build features

    `default:` yes
- 🟢 Use kleaf build system instead of build script:
    - Whenever to build using `kleaf (bazel)` instead of `build.sh`
    - Requires a different build configuration type (not starting with `build.config...`)

    `default:` no
- 🟢 Extra arguments that will be passed to build system/script
    - Input your extra arguments to pass to the build, if any
    - Examples: `BUILD_CONFIG_FRAGMENTS=` (`build.sh`), `--verbose_failures` (`kleaf`)
 
    `default:` `<none>` 
- 🟢 Space-separated list of build configurations:

    `default:` chickernel vanilla, ksu, ksu + susfs

# Notes
**A**ndroid **c**ommon **k**ernels above `common-android13-5.15` don't have `build.sh` support, 
and building without `kleaf` may not be possible. Make sure to select building with `kleaf` if that's the case.

By default, this GitHub workflow will retain artifacts for 30 days. You can increase or remove this expiration period 
if you need to by modifying the `retention-days` value in `build.yml`.

Also, this github workflow has reached the input limitation (10) and adding more 
is not possible traditionally (that's why AK3 has merged branch).
