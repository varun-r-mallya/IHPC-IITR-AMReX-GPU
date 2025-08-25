# Instructions to get up and running

Tested on Ubuntu Server 24.04 with Nvidia RTX 5070 16GB GPU running:
```bash
$ nvcc --version
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2023 NVIDIA Corporation
Built on Fri_Jan__6_16:45:21_PST_2023
Cuda compilation tools, release 12.0, V12.0.140
Build cuda_12.0.r12.0/compiler.32267302_0
```
```bash
.-/+oossssoo+/-.
`:+ssssssssssssssssss+:`
-+ssssssssssssssssssyyssss+-
.ossssssssssssssssssdMMMNysssso.
/ssssssssssshdmmNNmmyNMMMMhssssss/
+ssssssssshmydMMMMMMMNddddyssssssss+
/sssssssshNMMMyhhyyyyhmNMMMNhssssssss/
.ssssssssdMMMNhsssssssssshNMMMdssssssss.
+sssshhhyNMMNyssssssssssssyNMMMysssssss+   xeon@kratos
ossyNMMMNyMMhsssssssssssssshmmmhssssssso   -----------
ossyNMMMNyMMhsssssssssssssshmmmhssssssso   OS: Ubuntu 24.04.3 LTS x86_64
+sssshhhyNMMNyssssssssssssyNMMMysssssss+   Host: Z790 AORUS ELITE AX
.ssssssssdMMMNhsssssssssshNMMMdssssssss.   Kernel: 6.8.0-71-generic
/sssssssshNMMMyhhyyyyhdNMMMNhssssssss/    Uptime: 9 days, 3 hours, 10 mins
+sssssssssdmydMMMMMMMMddddyssssssss+     Packages: 1367 (dpkg), 6 (snap)
/ssssssssssshdmNNNNmyNMMMMhssssss/      Shell: bash 5.2.21
.ossssssssssssssssssdMMMNysssso.       Terminal: /dev/pts/7
-+sssssssssssssssssyyyssss+-         CPU: Intel i9-14900KS (32) @ 5.900GHz
`:+ssssssssssssssssss+:`           GPU: NVIDIA 04:00.0 NVIDIA Corporation Device 2c05
.-/+oossssoo+/-.               GPU: Intel Raptor Lake-S GT1 [UHD Graphics 770]
                               Memory: 4226MiB / 128572MiB
```
```bash
$ gcc --version
gcc (Ubuntu 13.3.0-6ubuntu2~24.04) 13.3.0
Copyright (C) 2023 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

Clone this repository with the `--submodule-recurse` flag, then install apptainer.

Add the following extra flag to `cmake` in amrex89: 
```bash
	cmake .. \
		-DCMAKE_BUILD_TYPE=Release \
		-DAMReX_GPU_BACKEND=CUDA \
		-DAMReX_CUDA_ARCH=89 \
		-DAMReX_MPI=ON \
		-DAMReX_OMP=ON \
		-DAMReX_EB=ON \
		-DAMReX_LINEAR_SOLVERS=ON \
		-DAMReX_PARTICLES=ON \
		-DAMReX_ASSERTIONS=ON \
		-DAMReX_TINY_PROFILE=ON \
		-DAMReX_PROFPARSER=ON \
		-DAMReX_HDF5=ON \
    -DAMReX_PIC=ON \
    -DAMReX_INSTALL=ON \   # This is the extra flag
		-DCMAKE_INSTALL_PREFIX=$(pwd)/../install
```

Then run the following two commands:
```bash
apptainer build cuda12.sif cuda12.def
apptainer build amrex_sm89.sif amrex_sm89.def
```
To `/path/to/A-STAR/IAMR/Tutorials/FlowPastCylinder/GNUmakefile`, add:
```make
AMREX_HOME       ?= /usr/local/amrex
AMREX_HYDRO_HOME ?= /usr/local/AMReX-Hydro
```
Then run from `A-STAR/IAMR/Tutorials/FlowPastCylinder/`:
```bash
cd IAMR/Tutorials/FlowPastCylinder;
apptainer exec --cleanenv --nv ../../../amrex_sm89.sif make -j32 USE_CUDA=TRUE USE_OMP=TRUE
```
For the cylinder flow, run from root directory:
```bash
apptainer exec --cleanenv --nv amrex_sm89.sif /home/xeon/A-STAR/IAMR/Tutorials/FlowPastCylinder/amr3d.gnu.MPI.OMP.CUDA.ex /home/xeon/A-STAR/cylinder_test/inputs.3d.flow_past_cylinder-x
```
