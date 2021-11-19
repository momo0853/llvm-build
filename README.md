# llvm-build
WebRTC本身是支持多平台，不过它在下载的时候是区分平台的，本身也不支持一个项目多平台编译（Linux、Windows和MacOS），稍微改动一下编译脚本就可以支持了，我们还需要把用到的所有第三方库都一起下载下来放到一个项目中。

# build 修改
修改`build/config/clang/clang.gni`文件，根据不同的平台选择不同的llvm，我们需要分别下载不同的平台的llvm，然后放到指定的位置。
```shell
# Copyright 2014 The Chromium Authors. All rights reserved.
# Use of this source code is governed by a BSD-style license that can be
# found in the LICENSE file.

import("//build/toolchain/toolchain.gni")

declare_args() {
  # Indicates if the build should use the Chrome-specific plugins for enforcing
  # coding guidelines, etc. Only used when compiling with Clang.
  clang_use_chrome_plugins = is_clang && !is_nacl && !use_xcode_clang

  #clang_base_path = "//third_party/llvm-build/Release+Asserts"
  if (host_os == "linux") {
    clang_base_path = "//third_party/llvm-build/linux/Release+Asserts"
  } else if (host_os == "mac") {
    clang_base_path = "//third_party/llvm-build/macos/Release+Asserts"
  } else if (host_os == "win") {
    clang_base_path = "//third_party/llvm-build/win/Release+Asserts"
  }
}
```
