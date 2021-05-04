# della-gpu-A100

## Get Samples

```
$ ssh della-gpu
$ cd /scratch/gpfs/jdh4
$ /usr/local/cuda-11.3/bin/cuda-install-samples-11.3.sh .
$ cd NVIDIA_CUDA-11.3_Samples/1_Utilities/bandwidthTest
$ module load cudatoolkit/11.3
$ make
$ sbatch job.slurm
# ./bandwidthTest --device=all --memory=pinned --mode=range --start=1000000 --end=10000000 --increment=1000000 --htod --dtoh --dtod

============= TIGER ============= 
Host to Device Bandwidth, 1 Device(s)
 PINNED Memory Transfers
   Transfer Size (Bytes) Bandwidth(GB/s)
   1000000 12.1
  10000000 12.4
 100000000 12.4
1000000000 12.4

Device to Host Bandwidth, 1 Device(s)
 PINNED Memory Transfers
   Transfer Size (Bytes) Bandwidth(GB/s)
   1000000 12.8
  10000000 13.1
 100000000 13.1
1000000000 13.1

Device to Device Bandwidth, 1 Device(s)
 PINNED Memory Transfers
   Transfer Size (Bytes) Bandwidth(GB/s)
   1000000 551.5
  10000000 460.2
 100000000 516.3
1000000000 522.6


============= DELLA =============
Host to Device Bandwidth, 1 Device(s)
 PINNED Memory Transfers
   Transfer Size (Bytes) Bandwidth(GB/s)
   1000000 18.0
  10000000 19.4
 100000000 24.1
1000000000 25.8

Device to Host Bandwidth, 1 Device(s)
 PINNED Memory Transfers
   Transfer Size (Bytes) Bandwidth(GB/s)
   1000000 15.8
  10000000 25.4
 100000000 26.2
1000000000 25.7

 Device to Device Bandwidth, 1 Device(s)
 PINNED Memory Transfers
   Transfer Size (Bytes) Bandwidth(GB/s)
   1000000 555.5
  10000000 1602.7
 100000000 1268.0
1000000000 1251.4
```

## Topology

```
#SBATCH --gres=gpu:2             # number of gpus per node
#SBATCH --time=00:01:00          # total run time limit (HH:MM:SS)

./topologyQuery
```

Output:

```
GPU0 <-> GPU1:
  * Atomic Supported: no
  * Perf Rank: 0
GPU1 <-> GPU0:
  * Atomic Supported: no
  * Perf Rank: 0
GPU0 <-> CPU:
  * Atomic Supported: no
GPU1 <-> CPU:
  * Atomic Supported: no
```

Another approach:

```
$ ssh della-i14g20
$ nvidia-smi topo -m
	GPU0	GPU1	mlx5_0	mlx5_1	CPU Affinity	NUMA Affinity
GPU0	 X 	SYS	NODE	SYS	0-63	0
GPU1	SYS	 X 	SYS	NODE	64-127	1
mlx5_0	NODE	SYS	 X 	SYS		
mlx5_1	SYS	NODE	SYS	 X 		

Legend:

  X    = Self
  SYS  = Connection traversing PCIe as well as the SMP interconnect between NUMA nodes (e.g., QPI/UPI)
  NODE = Connection traversing PCIe as well as the interconnect between PCIe Host Bridges within a NUMA node
  PHB  = Connection traversing PCIe as well as a PCIe Host Bridge (typically the CPU)
  PXB  = Connection traversing multiple PCIe bridges (without traversing the PCIe Host Bridge)
  PIX  = Connection traversing at most a single PCIe bridge
  NV#  = Connection traversing a bonded set of # NVLinks
```

Traverse for reference:

```
$ nvidia-smi topo -m
	GPU0	GPU1	GPU2	GPU3	mlx5_0	mlx5_1	mlx5_2	mlx5_3	CPU Affinity	NUMA Affinity
GPU0	 X 	NV3	SYS	SYS	NODE	NODE	SYS	SYS	0-63	0
GPU1	NV3	 X 	SYS	SYS	NODE	NODE	SYS	SYS	0-63	0
GPU2	SYS	SYS	 X 	NV3	SYS	SYS	NODE	NODE	64-127	8
GPU3	SYS	SYS	NV3	 X 	SYS	SYS	NODE	NODE	64-127	8
mlx5_0	NODE	NODE	SYS	SYS	 X 	PIX	SYS	SYS		
mlx5_1	NODE	NODE	SYS	SYS	PIX	 X 	SYS	SYS		
mlx5_2	SYS	SYS	NODE	NODE	SYS	SYS	 X 	PIX		
mlx5_3	SYS	SYS	NODE	NODE	SYS	SYS	PIX	 X 		

Legend:

  X    = Self
  SYS  = Connection traversing PCIe as well as the SMP interconnect between NUMA nodes (e.g., QPI/UPI)
  NODE = Connection traversing PCIe as well as the interconnect between PCIe Host Bridges within a NUMA node
  PHB  = Connection traversing PCIe as well as a PCIe Host Bridge (typically the CPU)
  PXB  = Connection traversing multiple PCIe bridges (without traversing the PCIe Host Bridge)
  PIX  = Connection traversing at most a single PCIe bridge
  NV#  = Connection traversing a bonded set of # NVLinks
```
