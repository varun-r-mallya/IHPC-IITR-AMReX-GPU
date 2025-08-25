# apptainer exec --cleanenv --nv --bind /scratch2/gauravk:/mnt/workdir /scratch2/gauravk/apptainer/amrex.sif make -j
#AMREX_HOME defines the directory in which we will find the AMReX directory
AMREX_HOME       = /usr/local/amrex
AMREX_HYDRO_HOME = /usr/local/AMReX-Hydro

#TOP defines the directory in which we will find Source, Exec, etc.
TOP = /mnt/workdir/AMReX/IAMR

#
# Variables for the user to set ...
#

PRECISION   = DOUBLE

# running 2d inputs files requires a 2d executable set here
DIM         = 3
COMP        = gnu

DEBUG       = FALSE
USE_MPI     = FALSE
USE_CUDA    = TRUE
USE_OMP     = FALSE
PROFILE     = FALSE


USE_VELOCITY = FALSE

USE_EB = TRUE

EBASE = amr

Blocs   := .

include $(TOP)/Exec/Make.IAMR
