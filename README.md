# FLASH

FLASH (Fast LSH Algorithm for Similarity Search Accelerated with HPC) is a library for large scale approximate nearest neighbor search of sparse vectors. It is currently available in C++ for CPU parallel computing and supports OpenCL enabled GPGPU computing. See [our paper](https://arxiv.org/pdf/1709.01190.pdf) for theoretical and benchmarking details. 

## Performance

We tested our system on a few large scale sparse datasets including [url](https://www.csie.ntu.edu.tw/~cjlin/libsvmtools/datasets/binary.html#url), [webspam](https://www.csie.ntu.edu.tw/~cjlin/libsvmtools/datasets/binary.html#webspam) and [kdd12](https://www.csie.ntu.edu.tw/~cjlin/libsvmtools/datasets/binary.html#kdd2012). 

The following results are from a head-to-head comparison with [NMSLIB](https://github.com/searchivarius/nmslib) v1.6 hnsw, one of the best methods available (see [ann-benchmarks](https://github.com/erikbern/ann-benchmarks)) on these datasets. In particular, we compared the timing for the construction of full knn-graph from grounds up, and the per-query timing (after building the index). We also estimated and compared the memory consumption of the index. 

**Webspam, Url**

![webspam_url_table](https://github.com/RUSH-LAB/Flash/blob/master/plots/webspam_url_table.PNG)
![webspam_url_plots](https://github.com/RUSH-LAB/Flash/blob/master/plots/allplots.PNG)

**Kdd2012**

![kdd_table](https://github.com/RUSH-LAB/Flash/blob/master/plots/kdd_table.PNG)

## Prerequisites

The current version of the soft is tested on 64-bit machines running Ubuntu 16.04, with CPU and at least 1 GPGPU installed. The compiler needs to support C++11 and OpenMP. 

### GPGPU

An active installation of OpenCL 1.1 or OpenCL 2.0 is required to support the GPGPU capability. OpenCL on Nvidia graphics cards requires the installation of [CUDA](https://developer.nvidia.com/cuda-toolkit-32-downloads). 

Then, install clinfo by `apt-get install clinfo` and verify the number of OpenCL platforms and devices. These information will be used in the configurations. 

## Configuration and Compilation

Navigate to the FLASH directory. Configure the system by editing the following section of *LSHReservoir_config.h* guided by the comments: 

```
// Comment out if not using GPU. 
#define USE_GPU
// Comment out if using OpenCL 1.XX. Does not matter if not usng GPU. 
#define OPENCL_2XX

#define CL_GPU_PLATFORM 0 // Does not matter if not usng OpenCL-GPU. 
#define CL_CPU_PLATFORM 1 // Does not matter if not usng OpenCL-CPU. 
#define CL_GPU_DEVICE 0 // Does not matter if not usng OpenCL-GPU. 
#define CL_CPU_DEVICE 0 // Does not matter if not usng OpenCL-CPU. 
```

Fill in CL_GPU_PLATFORM / CL_GPU_PLATFORM according to the order that the cpu/gpu platforms appear in the output of clinfo (make sure that for gpu is correct, OpenCL for cpu is currently experimental and is not required). If multiple devices exist, fill in CL_GPU_DEVICE / CL_CPU_DEVICE to choose the desired device according to their order in the output of clinfo. 

Save and close the file. Type in terminal:

```
make
```

The compilation is complete if no errors appear. 

## Tutorial

The current example code in the main() function verifies one of the results presented in [our paper](https://arxiv.org/pdf/1709.01190.pdf) on the Webspam dataset. Download the dataset from [libsvm](https://www.csie.ntu.edu.tw/~cjlin/libsvmtools/datasets/binary.html#webspam), and rename the libsvm-format file as trigram.svm. Download the groundtruths from  this [link](https://github.com/wangyiqiu/webspam). Place the dataset and groundtruth files in the FLASH directory. Run the program from the terminal:

```
./runme
```

The test program builds multiple hash tables for the dataset and query 10,000 test vectors followed by quality evaluations. The program will run with console outputs, indicating the progress and performance. The parameters, such as L, K and R can be edited in main.cpp. A re-compilation is required after changing the parameters. Please note that the time for parsing the dataset from disk might take about 5-10 minutes. 

A basic [documentation](https://github.com/RUSH-LAB/Flash/doc.pdf) of the API generated by [doxygen](http://www.stack.nl/~dimitri/doxygen/) is available. Tutorial codes in main.cpp could be used as a reference. 

## Authors

- [Yiqiu Wang](https://github.com/wangyiqiu)
- [Anshumali Shrivastava](https://www.cs.rice.edu/~as143/)
- [Heejung Ryu](https://github.com/bluejay9676)

## License

This library is licensed under Apache-2.0. See LICENSE for more details. 

## Acknowledgments

* Rice University Sketching and Hashing Lab ([RUSH Lab](http://rush.rice.edu/index.html)) provided the computing platform for testing. 
