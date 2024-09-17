# Dardel installation notes

https://www.pdc.kth.se/hpc-services/computing-systems/about-the-dardel-hpc-system-1.1053338

# Software modules

```
ml purge
ml snic-env PDC/23.12 cpeGNU/23.12
ml buildtools/23.12 cmake/3.27.7 boost/1.83.0-gcc-f3q gmp/6.3.0 eigen/3.4.0
ml cray-hdf5
ml cray-fftw cray-python
```

# Triqs

https://github.com/TRIQS/triqs

## Clone

```bash
mkdir -p ~/dev/
cd ~/dev
git clone https://github.com/TRIQS/triqs
cd triqs
git checkout 3.3.x
```

## CMake flags

```
CXX=g++-12 CC=gcc-12 cmake \
  -DCMAKE_INSTALL_PREFIX=$HOME/apps/triqs \
  -DCMAKE_BUILD_TYPE=Release \
  -DCMAKE_CXX_FLAGS="-Ofast -funroll-loops" \
  \
  -DHDF5_NO_FIND_PACKAGE_CONFIG_FILE=True \
  -DHDF5_COMPILER_NO_INTERROGATE=True \
  -DHDF5_INCLUDE_DIRS="/opt/cray/pe/hdf5/1.12.2.9/gnu/12.3/include" \
  -DHDF5_LIBRARIES="/opt/cray/pe/hdf5/1.12.2.9/gnu/12.3/lib/libhdf5.so" \
  -DHDF5_HL_LIBRARIES="/opt/cray/pe/hdf5/1.12.2.9/gnu/12.3/lib/libhdf5_hl.so" \
  -DHDF5_C_LIBRARY="/opt/cray/pe/hdf5/1.12.2.9/gnu/12.3/lib/libhdf5.so" \
  -DHDF5_C_HL_LIBRARY="/opt/cray/pe/hdf5/1.12.2.9/gnu/12.3/lib/libhdf5_hl.so" \
  \
  -DBLAS_LIBRARIES="/opt/cray/pe/libsci/23.12.5/GNU/12.3/x86_64/lib/libsci_gnu.so" \
  -DLAPACK_LIBRARIES="/opt/cray/pe/libsci/23.12.5/GNU/12.3/x86_64/lib/libsci_gnu.so" \
  \
  -DMPI_CXX_LIBRARY="/opt/cray/pe/mpich/8.1.28/ofi/gnu/12.3/lib/libmpi.so" \
  -DMPI_CXX_LIB_NAMES="CXX" \
  -DMPI_CXX_ADDITIONAL_INCLUDE_DIRS="/opt/cray/pe/mpich/8.1.28/ofi/gnu/12.3/include" \
  ..
```

## Build

```bash
mkdir cbuild
cd cbuild
../cmake.sh
make -j 64
```

## Run tests as Slurm job

Contents of `~/dev/triqs/jobscript`

```bash
#!/usr/bin/env bash

#SBATCH -J triqs_ctest
#SBATCH -t 00-00:10:00
#SBATCH -N 1
#SBATCH --ntasks-per-node=1
#SBATCH --cpus-per-task=64
#SBATCH --exclusive
#SBATCH -p main
#SBATCH -A naiss202X-XX-XX
#SBATCH --output=stdout-%x.%j
#SBATCH --error=stderr-%x.%j

OMP_NUM_THREADS=1 ctest -j 64
```

Submit job and monitor progress

```bash
sbatch ../jobscript
tail -f std*
```

## Install and source triqs environment variables

```
make -j 64 install
source ~/apps/triqs/share/triqs/triqsvars.sh
```

# TPRF

https://github.com/TRIQS/tprf

## Clone

```bash
mkdir -p ~/dev/
cd ~/dev
git clone https://github.com/TRIQS/triqs
cd triqs
git checkout 3.3.x
```

## CMake flags

```bash
CXX=g++-12 CC=gcc-12 cmake \
  -DCMAKE_INSTALL_PREFIX=$HOME/apps/tprf \
  -DCMAKE_BUILD_TYPE=Release \
  -DCMAKE_CXX_FLAGS="-Ofast -funroll-loops" \
  ..
```

## Build

```bash
mkdir cbuild
cd cbuild
../cmake.sh
make -j 64
```

## Run tests as Slurm job

Contents of `~/dev/tprf/jobscript`

```bash
#!/usr/bin/env bash

#SBATCH -J tprf_ctest
#SBATCH -t 00-00:10:00
#SBATCH -N 1
#SBATCH --ntasks-per-node=1
#SBATCH --cpus-per-task=64
#SBATCH --exclusive
#SBATCH -p main
#SBATCH -A naiss202X-XX-XX
#SBATCH --output=stdout-%x.%j
#SBATCH --error=stderr-%x.%j

OMP_NUM_THREADS=1 ctest -j 64
```

Submit job and monitor progress

```bash
sbatch ../jobscript
tail -f std*
```

## Install and source triqs environment variables

```
make -j 64 install
source ~/apps/tprf/share/triqs_tprf/triqs_tprfvars.sh
```

# w2dynamics_interface

## CMake flags

```bash
CXX=g++-12 CC=gcc-12 FC=ftn cmake \
  -DCMAKE_INSTALL_PREFIX=$HOME/apps/w2dynamics_interface \
  -DCMAKE_BUILD_TYPE=Release \
  -DCMAKE_CXX_FLAGS="-Ofast -funroll-loops" \
  ..
```

```
source ~/apps/w2dynamics_interface/share/w2dynamics_interface/w2dynamics_interfacevars.sh
```
