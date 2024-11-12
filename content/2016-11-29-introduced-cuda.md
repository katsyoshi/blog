+++
path = "/blog/2016/11/29/introduced-cuda"
layout = "post"
title = "YARAKASHI CUDA編"
date = 2016-11-29
comments = true
categories = "diary cuda gpgpu"
+++

[昨日](/blog/2016/11/28/hello/)かいた奴でGTX 1050があるのでCUDAを導入してみた。

## CUDA導入

これは簡単で [ここ](https://developer.nvidia.com/cuda-downloads) から必要なパッケージをダウンロードしてきます。
今回は Linux x86_64 Ubuntu 14.04 runfile(local) を順に選択。
インストールはここでダウンロードしたrunfileを実行して、指示に従うだけです。
これが終ったら、SampleProjectもインストールされてるとおもうのでこのプロジェクトをコンパイルします。

```
$ chmod 555 ./cuda_8.0.44_linux.run
$ ./cuda_8.0.44_linux.run
:
:
$ cd NVIDIA_CUDA-8.0_Samples
$ make
```

でコンパイルが終了したらサンプルプログラムを実行します。とりあえず `deviceQuery` を実行します。

```
$ cd bin/x86_64/linux/release
$ ./deviceQuery
./deviceQuery Starting...

 CUDA Device Query (Runtime API) version (CUDART static linking)

cudaGetDeviceCount returned 38
-> no CUDA-capable device is detected
Result = FAIL
```

と出ます。どうみてもエラーですね

で、以下のようにrootで実行すると
```
$ sudo ./deviceQuery
./deviceQuery Starting...

 CUDA Device Query (Runtime API) version (CUDART static linking)

Detected 1 CUDA Capable device(s)

Device 0: "GeForce GTX 1050"
  CUDA Driver Version / Runtime Version          8.0 / 8.0
  CUDA Capability Major/Minor version number:    6.1
  Total amount of global memory:                 1965 MBytes (2060255232 bytes)
  ( 5) Multiprocessors, (128) CUDA Cores/MP:     640 CUDA Cores
  GPU Max Clock rate:                            1455 MHz (1.46 GHz)
  Memory Clock rate:                             3504 Mhz
  Memory Bus Width:                              128-bit
  L2 Cache Size:                                 1048576 bytes
  Maximum Texture Dimension Size (x,y,z)         1D=(131072), 2D=(131072, 65536), 3D=(16384, 16384, 16384)
  Maximum Layered 1D Texture Size, (num) layers  1D=(32768), 2048 layers
  Maximum Layered 2D Texture Size, (num) layers  2D=(32768, 32768), 2048 layers
  Total amount of constant memory:               65536 bytes
  Total amount of shared memory per block:       49152 bytes
  Total number of registers available per block: 65536
  Warp size:                                     32
  Maximum number of threads per multiprocessor:  2048
  Maximum number of threads per block:           1024
  Max dimension size of a thread block (x,y,z): (1024, 1024, 64)
  Max dimension size of a grid size    (x,y,z): (2147483647, 65535, 65535)
  Maximum memory pitch:                          2147483647 bytes
  Texture alignment:                             512 bytes
  Concurrent copy and kernel execution:          Yes with 2 copy engine(s)
  Run time limit on kernels:                     Yes
  Integrated GPU sharing Host Memory:            No
  Support host page-locked memory mapping:       Yes
  Alignment requirement for Surfaces:            Yes
  Device has ECC support:                        Disabled
  Device supports Unified Addressing (UVA):      Yes
  Device PCI Domain ID / Bus ID / location ID:   0 / 129 / 0
  Compute Mode:
     < Default (multiple host threads can use ::cudaSetDevice() with device simultaneously) >

deviceQuery, CUDA Driver = CUDART, CUDA Driver Version = 8.0, CUDA Runtime Version = 8.0, NumDevs = 1, Device0 = GeForce GTX 1050
Result = PASS
```
と出てきますので、パーミッションがなかったようです。
あとは簡単、CUDA使いたいユーザーにパーミッションつけたらおわりです。

## おわり
こんなのに3時間ほど時間を費しましたね。はい。
