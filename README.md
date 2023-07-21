![image](https://github.com/beaufortfrancois/webgpu-cross-platform-app/assets/634478/81579516-7390-4198-bb18-68e7f4cb34c3)

# WebGPU cross-platform app with CMake/Emscripten

This app is a <em>minimalistic</em> C++ example that shows how to use [WebGPU](https://gpuweb.github.io/gpuweb/) to build desktop and web apps from a single codebase. Under the hood, it uses WebGPU's [webgpu.h](https://github.com/webgpu-native/webgpu-headers/blob/main/webgpu.h) as a platform-agnostic hardware abstraction layer through a C++ wrapper called [webgpu_cpp.h](https://source.chromium.org/chromium/chromium/src/+/main:third_party/dawn/include/webgpu/webgpu_cpp.h).

On the web, the app is built against [Emscripten](https://emscripten.org/), which has [bindings](https://github.com/emscripten-core/emscripten/blob/main/src/library_webgpu.js) implementing webgpu.h on top of the JavaScript API. On specific platforms such as macOS or Windows, this project can be built against [Dawn](https://dawn.googlesource.com/dawn/), Chromium's cross-platform WebGPU implementation.

<p align="center" width="100%">
:warning: The webgpu.h and webgpu_cpp.h APIs are not yet stabilized. :warning: 
</p>

## Setup

```sh
# Clone repository and initialize submodules.
git clone --recurse-submodules https://github.com/beaufortfrancois/webgpu-cross-platform-app.git
cd webgpu-cross-platform-app/
```

## Requirements

<i>Instructions are for macOS; they will need to be adapted to work on Linux and Windows.</i>

```sh
# Make sure CMake and Emscripten are installed.
brew install cmake emscripten
```

## Specific platform build

```sh
# Build the app with CMake.
cmake -B build && cmake --build build -j4

# Run the app.
./build/app
```

## Web build

```sh
# Build the app with Emscripten.
emcmake cmake -B build-web && cmake --build build-web -j4

# Run a server.
npx http-server
```

```sh
# Open the web app.
open http://127.0.0.1:8080/build-web/app.html
```

### Debugging WebAssembly

When building the app, compile it with DWARF debug information included thanks to `emcmake cmake -DCMAKE_BUILD_TYPE=Debug -B build-web`. And make sure to install the [C/C++ DevTools Support (DWARF) Chrome extension](https://goo.gle/wasm-debugging-extension) to enable WebAssembly debugging in DevTools.

<img width="1112" alt="image" src="https://github.com/beaufortfrancois/webgpu-cross-platform-app/assets/634478/e82f2494-6b1a-4534-b9e3-0c04caeca96d">
