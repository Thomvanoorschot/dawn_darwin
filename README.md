# Dawn Native for macOS (Pre-built)

This repository contains a pre-built static library of [Google's Dawn Native](https://dawn.googlesource.com/dawn) for macOS (universal binary supporting both x86_64 and arm64 architectures).

## Version

Built from Dawn Native version **v20250618.223156**

## What is Dawn Native?

Dawn Native is Google's cross-platform implementation of the WebGPU standard, providing native access to modern graphics APIs like Metal, Vulkan, and D3D12.

## Build Process

This library was built using the following steps:

```bash
cp scripts/standalone.gclient .gclient
gclient sync
cmake -G Ninja \
  -S . -B out/macos-universal \
  -DBUILD_SHARED_LIBS=OFF \
  -DDAWN_FETCH_DEPENDENCIES=ON \
  -DDAWN_ENABLE_INSTALL=ON \
  -DCMAKE_BUILD_TYPE=Release \
  -DCMAKE_OSX_ARCHITECTURES="x86_64;arm64"
cmake --build out/macos-universal --config Release
```

The individual static libraries were then merged into a single `dawn.a` file using `libtool`.

## Usage

To use this pre-built library in your Zig project, add it as a dependency and link the static library:

```zig
if (example_project.rootModuleTarget().os.tag == .macos) {
    const dawn_darwin = b.dependency("dawn_darwin", .{});
    example_project.addObjectFile(dawn_darwin.path("dawn.a"));
}
```

Replace `example_project` with your actual project variable name.
