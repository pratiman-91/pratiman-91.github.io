# Installing WRF from scratch in an HPC using Intel Compilers
The Weather Research and Forecasting (WRF) Model is a mesoscale numerical weather prediction system utilized worldwide for operational forecasts and research purposes.

Here is the installation of the WRF 4.2.1 model using Intel compilers. It will be helpful to those who want to install WRF from scratch in an HPC environment. Some of the steps may not be required depending on your HPC environment.

## Required Libraries
1. zlib
2. HDF5
3. NetCDF
4. JasPer

Generally, these libraries should be their (except for Jasper). If you do not have those then install using the steps below or you directly start from Step 4:

### Setting up the environemnt for Intel compilers
```bash
export CC=icc
export FC=ifort
export F90=ifort
export CXX=icpc
```

### 1. Installing zlib
```bash
mkdir wrf_install_intel
cd wrf_install_intel/
wget https://zlib.net/zlib-1.2.11.tar.gz

tar xvf zlib-1.2.11.tar.gz
cd zlib-1.2.11/

./configure --prefix=/home/wrf/wrf_libs_intel/
make
make install
```

### 2. Installing HDF5
```bash
cd ../
wget https://hdf-wordpress-1.s3.amazonaws.com/wp-content/uploads/manual/HDF5/HDF5_1_12_0/source/hdf5-1.12.0.tar.gz
./configure --prefix=/home/wrf/wrf_libs_intel/ --with-zlib=/home/wrf/wrf_libs_intel/ --enable-fortran
make 
make install
```

### 3. Installing NetCDF
```bash
cd ../
wget https://www.unidata.ucar.edu/downloads/netcdf/ftp/netcdf-c-4.7.4.tar.gz
tar xvf netcdf-c-4.7.4.tar.gz 
cd netcdf-c-4.7.4/

export LD_LIBRARY_PATH=/home/wrf/wrf_libs_intel/lib
export LDFLAGS=-L/home/wrf/wrf_libs_intel/lib
export CPPFLAGS=-I/home/wrf/wrf_libs_intel/include
./configure --prefix=/home/wrf/wrf_libs_intel/ 
make
make install

cd ../
wget https://www.unidata.ucar.edu/downloads/netcdf/ftp/netcdf-fortran-4.5.3.tar.gz
tar xvf netcdf-fortran-4.5.3.tar.gz 
cd netcdf-fortran-4.5.3/
./configure --prefix=/home/wrf/wrf_libs_intel/ 
make
make install
```

### 4. Installing JasPer
```bash
cd ../
wget https://www.ece.uvic.ca/~frodo/jasper/software/jasper-1.900.29.tar.gz
tar xvf jasper-1.900.29.tar.gz
./configure --prefix=/home/wrf/wrf_libs_intel/ 
make
make install
```

## WRF Install
```bash
export NETCDF=/home/wrf/wrf_libs_intel/
export HDF5=/home/wrf/wrf_libs_intel/
wget https://github.com/wrf-model/WRF/archive/v4.2.1.tar.gz
tar xvf v4.2.1.tar.gz
cd WRF-4.2.1/
./configure 
```

### Output:

```bash

checking for perl5... no
checking for perl... found /usr/local/bin/perl (perl)
Will use NETCDF in dir: /home/wrf/wrf_libs_intel/
Will use HDF5 in dir: /home/wrf/wrf_libs_intel/
PHDF5 not set in environment. Will configure WRF for use without.
Will use 'time' to report timing information


If you REALLY want Grib2 output from WRF, modify the arch/Config.pl script.
Right now you are not getting the Jasper lib, from the environment, compiled into WRF.

------------------------------------------------------------------------
Please select from among the following Linux x86_64 options:

  1. (serial)   2. (smpar)   3. (dmpar)   4. (dm+sm)   PGI (pgf90/gcc)
  5. (serial)   6. (smpar)   7. (dmpar)   8. (dm+sm)   PGI (pgf90/pgcc): SGI MPT
  9. (serial)  10. (smpar)  11. (dmpar)  12. (dm+sm)   PGI (pgf90/gcc): PGI accelerator
 13. (serial)  14. (smpar)  15. (dmpar)  16. (dm+sm)   INTEL (ifort/icc)
                                         17. (dm+sm)   INTEL (ifort/icc): Xeon Phi (MIC architecture)
 18. (serial)  19. (smpar)  20. (dmpar)  21. (dm+sm)   INTEL (ifort/icc): Xeon (SNB with AVX mods)
 22. (serial)  23. (smpar)  24. (dmpar)  25. (dm+sm)   INTEL (ifort/icc): SGI MPT
 26. (serial)  27. (smpar)  28. (dmpar)  29. (dm+sm)   INTEL (ifort/icc): IBM POE
 30. (serial)               31. (dmpar)                PATHSCALE (pathf90/pathcc)
 32. (serial)  33. (smpar)  34. (dmpar)  35. (dm+sm)   GNU (gfortran/gcc)
 36. (serial)  37. (smpar)  38. (dmpar)  39. (dm+sm)   IBM (xlf90_r/cc_r)
 40. (serial)  41. (smpar)  42. (dmpar)  43. (dm+sm)   PGI (ftn/gcc): Cray XC CLE
 44. (serial)  45. (smpar)  46. (dmpar)  47. (dm+sm)   CRAY CCE (ftn $(NOOMP)/cc): Cray XE and XC
 48. (serial)  49. (smpar)  50. (dmpar)  51. (dm+sm)   INTEL (ftn/icc): Cray XC
 52. (serial)  53. (smpar)  54. (dmpar)  55. (dm+sm)   PGI (pgf90/pgcc)
 56. (serial)  57. (smpar)  58. (dmpar)  59. (dm+sm)   PGI (pgf90/gcc): -f90=pgf90
 60. (serial)  61. (smpar)  62. (dmpar)  63. (dm+sm)   PGI (pgf90/pgcc): -f90=pgf90
 64. (serial)  65. (smpar)  66. (dmpar)  67. (dm+sm)   INTEL (ifort/icc): HSW/BDW
 68. (serial)  69. (smpar)  70. (dmpar)  71. (dm+sm)   INTEL (ifort/icc): KNL MIC
 72. (serial)  73. (smpar)  74. (dmpar)  75. (dm+sm)   FUJITSU (frtpx/fccpx): FX10/FX100 SPARC64 IXfx/Xlfx

Enter selection [1-75] : 20
------------------------------------------------------------------------
Compile for nesting? (1=basic, 2=preset moves, 3=vortex following) [default 1]: 

Configuration successful! 
------------------------------------------------------------------------
testing for fseeko and fseeko64
fseeko64 is supported
------------------------------------------------------------------------
...
```

### Edit configure.wrf
Edit here using your favorite editor:
```
DM_FC           =       mpiifort
DM_CC           =       mpiicc
```

### Compile the code
```
./compile -j 4 em_real 2>&1 | tee compile.log
```

This step will take around 30-45 min to complete. Be patient!

#### Output:
```
--->                  Executables successfully built                  <---
 
-rwxrwxr-x 1 wrf wrf 40691640 Jul 30 12:35 main/ndown.exe
-rwxrwxr-x 1 wrf wrf 40572760 Jul 30 12:35 main/real.exe
-rwxrwxr-x 1 wrf wrf 40048888 Jul 30 12:35 main/tc.exe
-rwxrwxr-x 1 wrf wrf 44609360 Jul 30 12:35 main/wrf.exe
 
==========================================================================
```
Sometimes, you have again re-run the compile code, if you do not get this as the output.

## WPS Install
```bash
cd ../
wget https://github.com/wrf-model/WPS/archive/v4.2.tar.gz
tar xvf v4.2.tar.gz
cd WPS-4.2/

export WRF_DIR=../WRF-4.2.1/
export JASPERLIB=/home/wrf/wrf_libs_intel/lib/
export JASPERINC=/home/wrf/wrf_libs_intel/include/

./configure
```

#### Output:
------------------------------------------------------------------------
Please select from among the following supported platforms.

   1.  Linux x86_64, gfortran    (serial)
   2.  Linux x86_64, gfortran    (serial_NO_GRIB2)
   3.  Linux x86_64, gfortran    (dmpar)
   4.  Linux x86_64, gfortran    (dmpar_NO_GRIB2)
   5.  Linux x86_64, PGI compiler   (serial)
   6.  Linux x86_64, PGI compiler   (serial_NO_GRIB2)
   7.  Linux x86_64, PGI compiler   (dmpar)
   8.  Linux x86_64, PGI compiler   (dmpar_NO_GRIB2)
   9.  Linux x86_64, PGI compiler, SGI MPT   (serial)
  10.  Linux x86_64, PGI compiler, SGI MPT   (serial_NO_GRIB2)
  11.  Linux x86_64, PGI compiler, SGI MPT   (dmpar)
  12.  Linux x86_64, PGI compiler, SGI MPT   (dmpar_NO_GRIB2)
  13.  Linux x86_64, IA64 and Opteron    (serial)
  14.  Linux x86_64, IA64 and Opteron    (serial_NO_GRIB2)
  15.  Linux x86_64, IA64 and Opteron    (dmpar)
  16.  Linux x86_64, IA64 and Opteron    (dmpar_NO_GRIB2)
  17.  Linux x86_64, Intel compiler    (serial)
  18.  Linux x86_64, Intel compiler    (serial_NO_GRIB2)
  19.  Linux x86_64, Intel compiler    (dmpar)
  20.  Linux x86_64, Intel compiler    (dmpar_NO_GRIB2)
  21.  Linux x86_64, Intel compiler, SGI MPT    (serial)
  22.  Linux x86_64, Intel compiler, SGI MPT    (serial_NO_GRIB2)
  23.  Linux x86_64, Intel compiler, SGI MPT    (dmpar)
  24.  Linux x86_64, Intel compiler, SGI MPT    (dmpar_NO_GRIB2)
  25.  Linux x86_64, Intel compiler, IBM POE    (serial)
  26.  Linux x86_64, Intel compiler, IBM POE    (serial_NO_GRIB2)
  27.  Linux x86_64, Intel compiler, IBM POE    (dmpar)
  28.  Linux x86_64, Intel compiler, IBM POE    (dmpar_NO_GRIB2)
  29.  Linux x86_64 g95 compiler     (serial)
  30.  Linux x86_64 g95 compiler     (serial_NO_GRIB2)
  31.  Linux x86_64 g95 compiler     (dmpar)
  32.  Linux x86_64 g95 compiler     (dmpar_NO_GRIB2)
  33.  Cray XE/XC CLE/Linux x86_64, Cray compiler   (serial)
  34.  Cray XE/XC CLE/Linux x86_64, Cray compiler   (serial_NO_GRIB2)
  35.  Cray XE/XC CLE/Linux x86_64, Cray compiler   (dmpar)
  36.  Cray XE/XC CLE/Linux x86_64, Cray compiler   (dmpar_NO_GRIB2)
  37.  Cray XC CLE/Linux x86_64, Intel compiler   (serial)
  38.  Cray XC CLE/Linux x86_64, Intel compiler   (serial_NO_GRIB2)
  39.  Cray XC CLE/Linux x86_64, Intel compiler   (dmpar)
  40.  Cray XC CLE/Linux x86_64, Intel compiler   (dmpar_NO_GRIB2)

Enter selection [1-40] : 17
------------------------------------------------------------------------
Configuration successful. To build the WPS, type: compile
------------------------------------------------------------------------

### Compile the WPS
```bash
./compile 2>&1 | tee compile.log
```

#### Output:
```bash
ls -rlt
```

```
lrwxrwxrwx 1 wrf wrf    23 Jul 30 12:46 geogrid.exe -> geogrid/src/geogrid.exe
lrwxrwxrwx 1 wrf wrf    21 Jul 30 12:46 ungrib.exe -> ungrib/src/ungrib.exe
lrwxrwxrwx 1 wrf wrf    23 Jul 30 12:46 metgrid.exe -> metgrid/src/metgrid.exe
```

Congratulations! You have installed the WRF. 