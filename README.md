# della-gpu-A100

## Get Samples

```
ssh della-gpu
cd /scratch/gpfs/jdh4
/usr/local/cuda-11.3/bin/cuda-install-samples-11.3.sh .
cd NVIDIA_CUDA-11.3_Samples/1_Utilities/bandwidthTest
make
./bandwidthTest --device=all --memory=pinned --mode=range --start=100000000 --end=1000000000 --increment=100000000 --htod --dtoh --dtod
[CUDA Bandwidth Test] - Starting...

!!!!!Cumulative Bandwidth to be computed from all the devices !!!!!!

Running on...

 Device 0: NVIDIA A100-PCIE-40GB
 Range Mode

 Host to Device Bandwidth, 1 Device(s)
 PINNED Memory Transfers
   Transfer Size (Bytes)	Bandwidth(GB/s)
   100000000			22.7
   200000000			26.2
   300000000			26.2
   400000000			26.2
   500000000			26.2
   600000000			25.4
   700000000			25.7
   800000000			25.7
   900000000			25.7
   1000000000			25.9

 Device to Host Bandwidth, 1 Device(s)
 PINNED Memory Transfers
   Transfer Size (Bytes)	Bandwidth(GB/s)
   100000000			26.2
   200000000			26.2
   300000000			26.3
   400000000			26.2
   500000000			25.7
   600000000			25.4
   700000000			25.5
   800000000			25.6
   900000000			25.7
   1000000000			25.8

 Device to Device Bandwidth, 1 Device(s)
 PINNED Memory Transfers
   Transfer Size (Bytes)	Bandwidth(GB/s)
   100000000			1260.0
   200000000			1300.5
   300000000			1190.9
   400000000			1284.0
   500000000			1318.5
   600000000			1204.9
   700000000			1210.7
   800000000			1244.7
   900000000			1210.3
   1000000000			1226.1

Result = PASS

NOTE: The CUDA Samples are not meant for performance measurements. Results may vary when GPU Boost is enabled.
```

