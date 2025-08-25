# Instructions to get up and running



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
apptainer exec --cleanenv --nv ../../../amrex_sm89.sif make -j32 USE_CUDA=TRUE USE_OMP=TRUE
```
Unpack the Zip File for the cylinder flow, then run:
```bash
apptainer exec --cleanenv --nv amrex_sm89.sif /home/xeon/A-STAR/IAMR/Tutorials/FlowPastCylinder/amr3d.gnu.MPI.CUDA.ex  /home/xeon/A-STAR/cylinder_test/inputs.3d.flow_past_cylinder-x
```
