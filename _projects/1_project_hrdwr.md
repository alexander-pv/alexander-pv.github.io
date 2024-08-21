---
layout: page
title: eGPU for 'GPU poor'
description: eGPU TH3P4G3 configuration 
img: /assets/img/projects/hardware/egpu/egpu-1.jpg
importance: 1
category: hardware
related_publications: false
giscus_comments: true
---

Today it is challenging to make experiments locally with your own resources when you work with language modelling.
Not anybody can afford cloud computing. Thus, I tested an unpopular but relatively cheap configuration to enhance my laptop with external GPU.


There is a wonderful site [egpi.io](https://egpu.io/) with a lot of manuals and community ready to give advice.
If you have a laptop with `Thunderbolt 2/3` port, then it is highly likely to find working eGPU configuration.
Thus, you need only a mediator with PCIe support between `Thunderbolt` and GPU.
`egpu.io` contains a list of best eGPU platforms. I decided to make tests with `TH3P4G3`. It costs $120-140 and
looks like a small board with `PCIe X16` and a bunch of USB ports (type A, 1 type C for Thunderbolt connection, 1 to power anything).
Total cost consists of `GPU`, `eGPU board`, `power supply`. That's all.

``
Note, that eGPU configuration won't be able to compete with standard way of connecting GPU because of the Thunderbolt 2/3 bandwith limit.
However, Thunderbolt 4 can completely solve this issue later.
``

![eGPU](/assets/img/projects/hardware/egpu/egpu-1.jpg)

OS support:

* **Windows**: works like a charm. However, I had to install basic Thunderbolt Software since it was not present on my laptop. GPU can be connected 'on the fly'.
* **Linux**: laptop requires restart. Sometimes 2-3 restarts but everything works fine. To make it work I installed `nvidia-driver-xxx-open`. 
   The proprietary version of drivers could not find my external GPU.
*  **MacOS**: unfortunately, there is no support for Apple Silicon Macs.


Time for some tests:

For tests I used official [`cuda-samples`](https://github.com/NVIDIA/cuda-samples) repository.  My external eGPU is NVIDIA RTX3090.

**deviceQuery**
```angular2html
./deviceQuery Starting...

 CUDA Device Query (Runtime API) version (CUDART static linking)

Detected 2 CUDA Capable device(s)

Device 0: "NVIDIA GeForce RTX 3090"
  CUDA Driver Version / Runtime Version          12.4 / 12.4
  CUDA Capability Major/Minor version number:    8.6
  Total amount of global memory:                 24039 MBytes (25206325248 bytes)
  (082) Multiprocessors, (128) CUDA Cores/MP:    10496 CUDA Cores
  GPU Max Clock rate:                            1860 MHz (1.86 GHz)
  Memory Clock rate:                             9751 Mhz
  Memory Bus Width:                              384-bit
  L2 Cache Size:                                 6291456 bytes
  Maximum Texture Dimension Size (x,y,z)         1D=(131072), 2D=(131072, 65536), 3D=(16384, 16384, 16384)
  Maximum Layered 1D Texture Size, (num) layers  1D=(32768), 2048 layers
  Maximum Layered 2D Texture Size, (num) layers  2D=(32768, 32768), 2048 layers
  Total amount of constant memory:               65536 bytes
  Total amount of shared memory per block:       49152 bytes
  Total shared memory per multiprocessor:        102400 bytes
  Total number of registers available per block: 65536
  Warp size:                                     32
  Maximum number of threads per multiprocessor:  1536
  Maximum number of threads per block:           1024
  Max dimension size of a thread block (x,y,z): (1024, 1024, 64)
  Max dimension size of a grid size    (x,y,z): (2147483647, 65535, 65535)
  Maximum memory pitch:                          2147483647 bytes
  Texture alignment:                             512 bytes
  Concurrent copy and kernel execution:          Yes with 2 copy engine(s)
  Run time limit on kernels:                     No
  Integrated GPU sharing Host Memory:            No
  Support host page-locked memory mapping:       Yes
  Alignment requirement for Surfaces:            Yes
  Device has ECC support:                        Disabled
  Device supports Unified Addressing (UVA):      Yes
  Device supports Managed Memory:                Yes
  Device supports Compute Preemption:            Yes
  Supports Cooperative Kernel Launch:            Yes
  Supports MultiDevice Co-op Kernel Launch:      Yes
  Device PCI Domain ID / Bus ID / location ID:   0 / 11 / 0
  Compute Mode:
     < Default (multiple host threads can use ::cudaSetDevice() with device simultaneously) >

Device 1: "NVIDIA GeForce RTX 2060"
  CUDA Driver Version / Runtime Version          12.4 / 12.4
  CUDA Capability Major/Minor version number:    7.5
  Total amount of global memory:                 5743 MBytes (6022365184 bytes)
  (030) Multiprocessors, (064) CUDA Cores/MP:    1920 CUDA Cores
  GPU Max Clock rate:                            1200 MHz (1.20 GHz)
  Memory Clock rate:                             7001 Mhz
  Memory Bus Width:                              192-bit
  L2 Cache Size:                                 3145728 bytes
  Maximum Texture Dimension Size (x,y,z)         1D=(131072), 2D=(131072, 65536), 3D=(16384, 16384, 16384)
  Maximum Layered 1D Texture Size, (num) layers  1D=(32768), 2048 layers
  Maximum Layered 2D Texture Size, (num) layers  2D=(32768, 32768), 2048 layers
  Total amount of constant memory:               65536 bytes
  Total amount of shared memory per block:       49152 bytes
  Total shared memory per multiprocessor:        65536 bytes
  Total number of registers available per block: 65536
  Warp size:                                     32
  Maximum number of threads per multiprocessor:  1024
  Maximum number of threads per block:           1024
  Max dimension size of a thread block (x,y,z): (1024, 1024, 64)
  Max dimension size of a grid size    (x,y,z): (2147483647, 65535, 65535)
  Maximum memory pitch:                          2147483647 bytes
  Texture alignment:                             512 bytes
  Concurrent copy and kernel execution:          Yes with 3 copy engine(s)
  Run time limit on kernels:                     No
  Integrated GPU sharing Host Memory:            No
  Support host page-locked memory mapping:       Yes
  Alignment requirement for Surfaces:            Yes
  Device has ECC support:                        Disabled
  Device supports Unified Addressing (UVA):      Yes
  Device supports Managed Memory:                Yes
  Device supports Compute Preemption:            Yes
  Supports Cooperative Kernel Launch:            Yes
  Supports MultiDevice Co-op Kernel Launch:      Yes
  Device PCI Domain ID / Bus ID / location ID:   0 / 1 / 0
  Compute Mode:
     < Default (multiple host threads can use ::cudaSetDevice() with device simultaneously) >
> Peer access from NVIDIA GeForce RTX 3090 (GPU0) -> NVIDIA GeForce RTX 2060 (GPU1) : No
> Peer access from NVIDIA GeForce RTX 2060 (GPU1) -> NVIDIA GeForce RTX 3090 (GPU0) : No

deviceQuery, CUDA Driver = CUDART, CUDA Driver Version = 12.4, CUDA Runtime Version = 12.4, NumDevs = 2
Result = PASS
```


**Bandwidth** 
```angular2html
[CUDA Bandwidth Test] - Starting...
Running on...

 Device 1: NVIDIA GeForce RTX 2060
 Quick Mode

 Host to Device Bandwidth, 1 Device(s)
 PINNED Memory Transfers
   Transfer Size (Bytes)	Bandwidth(GB/s)
   32000000			12.8
 Device to Host Bandwidth, 1 Device(s)
 PINNED Memory Transfers
   Transfer Size (Bytes)	Bandwidth(GB/s)
   32000000			13.0
 Device to Device Bandwidth, 1 Device(s)
 PINNED Memory Transfers
   Transfer Size (Bytes)	Bandwidth(GB/s)
   32000000			286.9

Result = PASS


 Device 0: NVIDIA GeForce RTX 3090
 Quick Mode

 Host to Device Bandwidth, 1 Device(s)
 PINNED Memory Transfers
   Transfer Size (Bytes)	Bandwidth(GB/s)
   32000000			2.4
 Device to Host Bandwidth, 1 Device(s)
 PINNED Memory Transfers
   Transfer Size (Bytes)	Bandwidth(GB/s)
   32000000			2.7
 Device to Device Bandwidth, 1 Device(s)
 PINNED Memory Transfers
   Transfer Size (Bytes)	Bandwidth(GB/s)
   32000000			778.8

Result = PASS
```

Yes, the host-to-device and device-to-host bandwidth for eGPU is about x5 slower than
the graphics chip of the previous generation.

**bf16TensorCoreGemm**

According to the technical sheets RTX20 series (Turing) does not support bfloat16.

```angular2html
# Internal GPU
Initializing...
GPU Device 0: "Turing" with compute capability 7.5
(Nothing)


#  eGPU
Initializing...
GPU Device 0: "Ampere" with compute capability 8.6

M: 8192 (16 x 512)
N: 8192 (16 x 512)
K: 8192 (16 x 512)
Preparing data for GPU...
Required shared memory size: 72 Kb
Computing using high performance kernel = 0 - compute_bf16gemm_async_copy
Time: 19.137535 ms
TFLOPS: 57.45
```

**cudaTensorCoreGemm**

```angular2html
# Internal GPU
Initializing...
GPU Device 0: "Turing" with compute capability 7.5

M: 4096 (16 x 256)
N: 4096 (16 x 256)
K: 4096 (16 x 256)
Preparing data for GPU...
Required shared memory size: 64 Kb
Computing... using high performance kernel compute_gemm 
Time: 13.607328 ms
TFLOPS: 10.10

Initializing...
GPU Device 0: "Ampere" with compute capability 8.6

#  eGPU
M: 4096 (16 x 256)
N: 4096 (16 x 256)
K: 4096 (16 x 256)
Preparing data for GPU...
Required shared memory size: 64 Kb
Computing... using high performance kernel compute_gemm 
Time: 2.282496 ms
TFLOPS: 60.21
```
Despite the bandwidth issue, general-purpose computations are obviously faster.


**UnifiedMemoryPerf**

The performance comparision using matrix multiplication kernel of Unified Memory with/without hints 
and other types of memory like zero copy buffers, pageable, pagelocked memory 
performing synchronous and Asynchronous transfers on a single GPU


```angular2html
# Internal GPU
GPU Device 0: "Turing" with compute capability 7.5

Running ........................................................
Overall Time For matrixMultiplyPerf
Printing Average of 20 measurements in (ms)
Size_KB	 UMhint	UMhntAs	 UMeasy	  0Copy	MemCopy	CpAsync	CpHpglk	CpPglAs
4	  0.272	  0.212	  0.366	  0.017	  0.031	  0.024	  0.038	  0.029
16	  0.321	  0.231	  0.476	  0.033	  0.049	  0.038	  0.052	  0.054
64	  0.397	  0.299	  0.821	  0.093	  0.109	  0.101	  0.108	  0.104
256	  0.824	  0.719	  1.295	  0.607	  0.554	  0.484	  0.364	  0.352
1024	  2.556	  2.284	  3.219	  2.696	  1.762	  1.713	  1.442	  1.440
4096	  9.923	  8.923	 12.384	 19.356	  7.041	  7.165	  6.430	  6.353
16384	 41.472	 40.621	 56.780	163.723	 36.694	 36.624	 35.485	 35.467

# eGPU
GPU Device 0: "Ampere" with compute capability 8.6

Running ........................................................
Overall Time For matrixMultiplyPerf
Printing Average of 20 measurements in (ms)
Size_KB	 UMhint	UMhntAs	 UMeasy	  0Copy	MemCopy	CpAsync	CpHpglk	CpPglAs
4	  0.471	  0.362	  0.519	  0.036	  0.067	  0.045	  0.078	  0.060
16	  0.505	  0.377	  0.794	  0.072	  0.096	  0.073	  0.109	  0.090
64	  0.871	  0.694	  1.026	  0.304	  0.207	  0.182	  0.216	  0.184
256	  1.555	  1.397	  2.040	  2.014	  0.636	  0.595	  0.603	  0.572
1024	  4.550	  4.097	  5.197	 14.856	  2.463	  2.273	  2.153	  2.094
4096	 15.329	 14.123	 20.882	116.191	  9.167	  9.038	  8.838	  8.831
16384	 61.450	 56.328	 87.981	919.314	 39.508	 39.309	 39.976	 39.827
```

**alignedTypes**

It measures per-element copy throughput for aligned and misaligned structures on big chunks of data

```angular2html
[./alignedTypes] - Starting...
GPU Device 0: "Turing" with compute capability 7.5

[NVIDIA GeForce RTX 2060] has 30 MP(s) x 64 (Cores/MP) = 1920 (Cores)
> Compute scaling value = 1.00
> Memory Size = 49999872
Allocating memory...
Generating host input data array...
Uploading input data to GPU memory...
Testing misaligned types...
uint8...
Avg. time: 1.470281 ms / Copy throughput: 31.671498 GB/s.
	TEST OK
uint16...
Avg. time: 0.855563 ms / Copy throughput: 54.427361 GB/s.
	TEST OK
RGBA8_misaligned...
Avg. time: 0.598750 ms / Copy throughput: 77.772042 GB/s.
	TEST OK
LA32_misaligned...
Avg. time: 0.361500 ms / Copy throughput: 128.813306 GB/s.
	TEST OK
RGB32_misaligned...
Avg. time: 0.384937 ms / Copy throughput: 120.970314 GB/s.
	TEST OK
RGBA32_misaligned...
Avg. time: 0.353063 ms / Copy throughput: 131.891685 GB/s.
	TEST OK
Testing aligned types...
RGBA8...
Avg. time: 0.495594 ms / Copy throughput: 93.960041 GB/s.
	TEST OK
I32...
Avg. time: 0.494844 ms / Copy throughput: 94.102450 GB/s.
	TEST OK
LA32...
Avg. time: 0.352000 ms / Copy throughput: 132.289800 GB/s.
	TEST OK
RGB32...
Avg. time: 0.342406 ms / Copy throughput: 135.996380 GB/s.
	TEST OK
RGBA32...
Avg. time: 0.341781 ms / Copy throughput: 136.245064 GB/s.
	TEST OK
RGBA32_2...
Avg. time: 0.371625 ms / Copy throughput: 125.303757 GB/s.
	TEST OK

[alignedTypes] -> Test Results: 0 Failures
Shutting down...



[./alignedTypes] - Starting...
GPU Device 0: "Ampere" with compute capability 8.6

[NVIDIA GeForce RTX 3090] has 82 MP(s) x 128 (Cores/MP) = 10496 (Cores)
> Compute scaling value = 1.00
> Memory Size = 49999872
Allocating memory...
Generating host input data array...
Uploading input data to GPU memory...
Testing misaligned types...
uint8...
Avg. time: 0.955813 ms / Copy throughput: 48.718769 GB/s.
	TEST OK
uint16...
Avg. time: 0.495656 ms / Copy throughput: 93.948194 GB/s.
	TEST OK
RGBA8_misaligned...
Avg. time: 0.277250 ms / Copy throughput: 167.956757 GB/s.
	TEST OK
LA32_misaligned...
Avg. time: 0.164281 ms / Copy throughput: 283.452980 GB/s.
	TEST OK
RGB32_misaligned...
Avg. time: 0.148125 ms / Copy throughput: 314.369700 GB/s.
	TEST OK
RGBA32_misaligned...
Avg. time: 0.131437 ms / Copy throughput: 354.282539 GB/s.
	TEST OK
Testing aligned types...
RGBA8...
Avg. time: 0.267969 ms / Copy throughput: 173.774034 GB/s.
	TEST OK
I32...
Avg. time: 0.268000 ms / Copy throughput: 173.753763 GB/s.
	TEST OK
LA32...
Avg. time: 0.164156 ms / Copy throughput: 283.668830 GB/s.
	TEST OK
RGB32...
Avg. time: 0.130969 ms / Copy throughput: 355.550539 GB/s.
	TEST OK
RGBA32...
Avg. time: 0.130750 ms / Copy throughput: 356.145387 GB/s.
	TEST OK
RGBA32_2...
Avg. time: 0.127000 ms / Copy throughput: 366.661481 GB/s.
	TEST OK

[alignedTypes] -> Test Results: 0 Failures
Shutting down...
Test passed
```