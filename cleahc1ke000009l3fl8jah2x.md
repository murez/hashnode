# Paper Review of Symmetric Indefinite Linear Solver using OpenMP Task on Multicore
  Architectures

## Paper Summary

This paper provides a portable high-performance symmetric-indefinite linear system solver features OpenMP's innovative dataflow-based task scheduling and Aasen's method to avoid communication. This work is now available as a part of the PLASMA software package.

## Paper Details

### Algorithm

PLASMA employs a unique matrix architecture (store the matrix in tiles, only store triangular matrix for symmetric ones and using band matrix) to take use of current processors' memory hierarchy and produce a sufficient number of separate jobs that adhere to the OpenMP standard.

And by using the OpenMP task, PLASMA can parallelize the computing tasks without caring about the dependency problem.

The PLASMA consists of two parts the Factorization and Solve.

1. Factorization
    
    1. Aasen's LTLT factorization with LU panel factorization
        
    2. Band LU factorization
        
2. 1. Forward substitution with the lower-triangular L-factor from Aasen's factorization, interleaved with the row pivoting
        
    2. Forward substitution with the L-factor from the band LU, interleaved with the partial pivoting
        
    3. Backward substitution with the upper-triangular U-factor from the band LU
        
    4. Backward substitution with the U-factor from Aasen's factorization
        
    5. Row interchange of the solution vectors with column pivoting from Aasen's factorization
        

### Experiment

The critical point of this paper is the portability of PLASMA. The experinments show the equivalent high performance on several CPU architects including Intel's Broadwell, Intel's Knights Landing (KNL), IBM's Power8, and Arm's ARMv8. And PLASMA has the same performance and better scalibilty compared with other tranditional Linear solvers like LAPACK, MKL.

## My thoughts and Brain Storm

### Multi-platform shared-memory parallel programming lib

The usage of openmp in parallel computing applications has made a major impact with the advent of multi-core processors on different platforms and the improvement in computational speed. However, many functionalities remain unimplemented in openmp and other frameworks such as cilk and quark. For example, I used openmp to build parallelized bfs in a difficult method to emulate what cilk can accomplish in a simple way, and there is a significant speed loss. However, the benefit of openmp is that it is cross-platform compatible. I expect that as openmp improves, more parallel applications will adopt it to gain cross-platform compatibility.

### Computing Framework and Hardware Updates

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676756148638/edf2580a-5a7c-4339-8bfe-2edd11e8dadf.png align="center")

Of course, the creation of these linpacks is inextricably linked to the updating of the hardware design. Initially, OpenBLAS, LAPACK, and LINPACK provided common standards; later, ScaLAPACK emerged when multi-node communication was required; MKL emerged following the emergence of simd CPU; PLASMA emerged following the popularity of multi-core hyper-threaded platforms; and now, Cublas emerged following the popularity of heterogeneous computing and GPGPU. SLATE also evolved to combine all Cuda/HIP, 'OpenMP, MPI in order to allow the integration of all computing resources. Of course, this also applies to other devices, such as DPU, which generated Nvidia's Doca framework. With the develop of the speed on storage devices, the file system is going to have a revolution like the computing libs.