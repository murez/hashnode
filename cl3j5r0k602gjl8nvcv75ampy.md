## How to profile a Slurm/MPI program on Niagra/Bridge-2 by Intel Vtune

---
tags: Intel, Profile, MPI
---



## Build Program

Here are two choices:
1. build program with whatever mpi you want.
2. build program with IntelMPI

## Modify Sbatch Script

### add paths and set env

``` bash
module load intel/2019u4 ...
. $INTEL_ROOT/vtune_amplifier/amplxe-vars.sh

```

### mpirun + vtune command + your program

like following:
``` bash
srun amplxe-cl -collect hotspots {-trace-mpi} -data-limit=0 -r $WHERE_SAVE_REPORT $YOUR_PROGRAM

# or 
mpirun -np xxx amplxe-cl -collect hotspots {-trace-mpi} -data-limit=0 -r $WHERE_SAVE_REPORT $YOUR_PROGRAM

```

`-trace-mpi` is only required by no intelmpi program.

for `collect` we have `hotspots`, `hpc-performance` and so on. Please check [Intel-Vtune-Doc](https://www.intel.com/content/www/us/en/develop/documentation/vtune-help/top/analyze-performance/algorithm-group/basic-hotspots-analysis.html) for more usages. 


## Visualize through X11-Forwarding

Mobaxterm have X11-Forwarding embeded.

Make sure that your ssh conecting has X11-Forwarding Access.

``` bash
$INTEL_ROOT/vtune_amplifier/bin64/amplxe-gui --allow-remote-ui --web-port $NOT_USED_PORT --path-to-open  $WHERE_SAVE_REPORT

```



Then you can have a vision of the profiling results.

### problem fix

#### `Failed to launch VTune Amplifier GUI...`

I solved the problem by trying another `$NOT_USED_PORT`. It works well on Niagara HPC. And please be aware of your connection lagency.


## Visualize through vtune-backend

This method has been tested on openapi@2021u4  on Niagara HPC.

``` bash
vtune-backend --web-port=$port --allow-remote-ui --data-directory $where_you_can_find_your_vtune_results

```

then forwarding the port to your local address and open with `https`(oneapi is required to use `tls`).

## Great Help Book
[NERSC DOC](https://docs.nersc.gov/tools)

[PSC Bridges-2 User Guide](https://www.psc.edu/resources/bridges-2/user-guide-2-2/)

[Niagara_Quickstart](https://docs.scinet.utoronto.ca/index.php/Niagara_Quickstart)