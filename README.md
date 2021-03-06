# cpu_check

`NOTE:` BETA code, work-in-progress.

CPU torture test designed for SMP systems, attempting to find CPU hardware faults, focusing primarily on the x86_64 architecture.

The basic operation is to:
* Run threads with affinity fixed to each logical CPU.
* Generate a chunk of random data, either dictionary based text, or random binary data.
* Run a number of checksum/hash algorithms over the data, store their results.
* Compress the data via one of (zlib, ...).
* Encrypt the data via AES-256-GCM.
* Copy the data ('rep movsb' on x86, else memcpy).
* Switch affinity to an alternate logical CPU.
* Decrypt.
* Decompress.
* Run checksum/hash algorithms and compare with stored results.

Algorithms are chosen to exercise various hardware extensions. Eg. on x86_64, SSE4.2, AVX, etc.

## Prerequisites:

Designed to run under Unix/Linux OS. There is a preference for the Clang/LLVM compiler, as I've found unexplained SEGV when built with gcc 5.5 and -O2 on x86.

* cmake: https://cmake.org/
* zlib
* OpenSSL

## Building

```
sh$ git clone git@github.com:stixpjr/cpu_check.git
sh$ cd cpu_check
sh$ mkdir build
sh$ cd build
sh$ cmake ..
sh$ make
```

## TODO:

* Use git submodules for:
  * farmhash: https://github.com/google/farmhash
  * highwayhash: https://github.com/google/highwayhash
  * crc32c: https://github.com/google/crc32c
  * cityhash: https://github.com/google/cityhash
  * brotli: https://github.com/google/brotli
  * gipfeli: https://github.com/google/gipfeli
* Expand encryption coverage - find those algorithms that stress the HW.
* Flags to enable/disable steps, eg. encryption.
* Flags controlling min/max buffer size.
* Use cpuid to dynamically select appropriate instruction set extensions.
