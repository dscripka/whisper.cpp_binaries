# Overview

A collection of scripts and pre-built binaries for the [whisper.cpp project](https://github.com/ggerganov/whisper.cpp).

Binaries are included in releases, which track [whisper.cpp releases or commits](https://github.com/ggerganov/whisper.cpp).

All binaries are statically built and should (in theory) run without issue on the appropriate architecture, platform, and hardware.

Currently, the following builds are included in each release:

| Name | Build Options | Architecture | Platform |
|---|---|---|---|
| whisper-bin-linux | None | x64 | linux |
| whisper-bin-minimal-linux | WHISPER_NO_AVX2, WHISPER_NO_FMA, WHISPER_NO_F16C | x64 | linux |
| whisper-bin-blas-linux | WHISPER_OPENBLAS | x64 | linux |
| whisper-bin-blas-minimal-linux | WHISPER_OPENBLAS, WHISPER_NO_AVX2, WHISPER_NO_FMA, WHISPER_NO_F16C | x64 | linux |

# Details

If the pre-compiled binaries do not work on your system, you can re-build them with `cmake` as shown below.

## Get whisper.cpp

```bash
# Get whisper.cpp
git clone https://github.com/ggerganov/whisper.cpp


# Update CMakeLists.txt for whisper.cpp
cd whisper.cpp
sed -i '4i\set(CMAKE_FIND_LIBRARY_SUFFIXES ".a" ".so")' CMakeLists.txt
```

Note that `cmake` and certain other dependencies need to be installed to build whisper.cpp, depending on the build options. See the [whisper.cpp README](https://github.com/ggerganov/whisper.cpp) for more information.

## Build whisper.cpp with CMake

### Standard Build

```bash
mkdir build
cmake -B build -DCMAKE_EXE_LINKER_FLAGS="-static -static-libgcc -static-libstdc++" -DBUILD_SHARED_LIBS=OFF
cmake --build build --config release
```

### With OpenBLAS

```bash
mkdir build
cmake -B build -DCMAKE_EXE_LINKER_FLAGS="-static -static-libgcc -static-libstdc++" -DWHISPER_OPENBLAS=1 -DBUILD_SHARED_LIBS=OFF
cmake --build build --config release
```

### Minimal Build (suitable for old hardware)

```bash
cmake -B build -DCMAKE_EXE_LINKER_FLAGS="-static -static-libgcc -static-libstdc++" -DBUILD_SHARED_LIBS=OFF -DWHISPER_NO_AVX2=ON -DWHISPER_NO_FMA=ON -DWHISPER_NO_F16C=ON
```

### Minimal Build with OpenBLAS (suitable for old hardware)

```bash
cmake -B build -DWHISPER_OPENBLAS=ON -DCMAKE_EXE_LINKER_FLAGS="-static -static-libgcc -static-libstdc++" -DBUILD_SHARED_LIBS=OFF -DWHISPER_NO_AVX2=ON -DWHISPER_NO_FMA=ON -DWHISPER_NO_F16C=ON
```

### Testing

After building, use this to test (after downloading the model):

```bash
./build/bin/main -m models/ggml-tiny.en.bin -f samples/jfk.wav -t 2 -bs 1
```