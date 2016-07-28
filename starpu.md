# Installation of StarPU on MacOS (using HomeBrew)

## 1. Install the homebrew packages:

	brew install pkg-config
	brew install automake
	brew install doxygen
	brew install hwloc
	brew install open-mpi

## 2. Install StarPU

### 2.1 Download

Last version tarball is downloadable at [http://starpu.gforge.inria.fr/files](http://starpu.gforge.inria.fr/files) 

### 2.2 Compile

#### Option 1: using `gcc-5`

Install a version of gcc that supports OpenMP:

	brew reinstall gcc --without-multilib

##### ``QMAKESPEC`` variable

For Qmake installed with the Python distribution [Anaconda](https://www.continuum.io/downloads), the following environment variable must be set:

	export QMAKESPEC=macx-g++

##### Run the `configure` script :

	cd ~/Downloads/starpu-1.1.5
	../configure --prefix=/opt/starpu --enable-mpi-check --disable-build-doc CPPFLAGS=-I/opt/X11/include/X11 LDFLAGS="-L/opt/X11/lib/ -L/usr/local//lib/gcc/5" CC=/usr/local/bin/gcc-5 -with-hwloc=/usr/local --disable-socl --disable-opencl --enable-openmp
	
Without ``--disable-build-doc``, the configure is OK but compiling the html doc fails. Anyway, this documentation is available [on line](http://starpu.gforge.inria.fr/files/doc/starpu-1.1.5/html/index.html).


#### Option 2: using `clang`

##### Option 2.1: with OpenMP support

Install clang with homebrew (native clang provided by Apple is not enough)

	brew install clang-omp

###### Run the configure script:

	./configure --prefix=/opt/starpu --enable-opencl --enable-mpi-check \
	--enable-opencl-check --disable-build-doc CPPFLAGS=-I/opt/X11/include/X11 \
	LDFLAGS=-L/opt/X11/lib/ CC=/usr/local/bin/clang-omp CFLAGS=-std=gnu99

##### Option 2.2: without OpenMP support

###### Run the configure script:

	./configure --prefix=/opt/starpu --enable-opencl --enable-mpi-check \
	--enable-opencl-check --disable-build-doc CPPFLAGS=-I/opt/X11/include/X11 \
	LDFLAGS=-L/opt/X11/lib/ CFLAGS=-std=gnu99

#### Finally compile:
 
	make -j4


### 2.4 Check the test-cases execution:

	make check

### 2.5 Install StarPU

	sudo make install
	
To link other applications with StarPU libraries, add the following line to your `~/.bashrc` file:

	export PGK_CONFIG_PATH=/opt/starpu/lib/pkgconfig:$PGK_CONFIG_PATH
