# NCCL 

NCCL (pronounced "Nickel") is a stand-alone library of standard communication routines for GPUs, implementing all-reduce, all-gather, reduce, broadcast, reduce-scatter, as well as any send/receive based communication pattern. It has been optimized to achieve high bandwidth on platforms using PCIe, NVLink, NVswitch, as well as networking using InfiniBand Verbs or TCP/IP sockets. NCCL supports an arbitrary number of GPUs installed in a single node or across multiple nodes, and can be used in either single- or multi-process (e.g., MPI) applications.

NCCL is a key library for scaling AI applications on Nvidia system. The conda module on Polaris are built with NCCL as the communication backend for distributed training. But HPC applications can also chose NCCL for communication over MPI. The library is available in the following folder: ```/soft/libraries/nccl```. 

We have done extensive performance tests and identified the following best environment setup. 

```bash
export NCCL_NET_GDR_LEVEL=PHB
export NCCL_CROSS_NIC=1
export NCCL_COLLNET_ENABLE=1
export NCCL_NET="AWS Libfabric"
export LD_LIBRARY_PATH=/soft/libraries/aws-ofi-nccl/v1.9.1-aws/lib:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=/soft/libraries/hwloc/lib/:$LD_LIBRARY_PATH
export FI_CXI_DISABLE_HOST_REGISTER=1
export FI_MR_CACHE_MONITOR=userfaultfd
export FI_CXI_DEFAULT_CQ_SIZE=131072
```
This setup will lead to 2-3x performance improvement. For details, please refer to: https://github.com/argonne-lcf/alcf-nccl-tests. 

As of now (October 29, 2029), these setup has been included in the conda module on Polaris as one can confirm as follows: 
```bash 
module load conda
env | grep NCCL
env | grep FI
```
