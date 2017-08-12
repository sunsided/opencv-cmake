# OpenCV with Contributions for CMake

## Cloning and initialization

Clone the repository in `--recursive` mode to fetch the submodules

```bash
git clone --recursive https://github.com/sunsided/opencv-cmake.git
```

If you didn't, pull the submodules using

```bash
git submodule update --init --recursive
```

## Custom builds

Build from the `build` subdirectory. Note that the paths specified are
considered to be examples taken from my setup. Adjust them accordingly to fit your system.

For Anaconda builds, in order to obtain the correct Python paths, [this page](https://www.scivision.co/anaconda-python-opencv3/) has some nice tricks:

* `CMAKE_INSTALL_PREFIX`: `python3 -c "import sys; print(sys.prefix)"`
* `PYTHON3_EXECUTABLE`: `which python3`
* `PYTHON3_INCLUDE_DIR`: `python3 -c "from distutils.sysconfig import get_python_inc; print(get_python_inc())"`
* `PYTHON3_PACKAGES_PATH`: `python3 -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())"`

### Ubuntu 17.04 + CUDA 8 (GTX 980 Ti)

Targeting Anaconda 4's Python 3.6. For some reason, CMake doesn't detect
the include path unless the `PYTHON3_INCLUDE_PATH` variable (note it's `_PATH`,
not `_DIR`) is set.
It then also seems to expect `PYTHON3_LIBRARIES` (rather than `PYTHON3_LIBRARY`).

```bash
cmake \
    -DCMAKE_BUILD_TYPE=RELEASE \
    -DCMAKE_INSTALL_PREFIX="/home/markus/anaconda3" \
    -DOPENCV_EXTRA_MODULES_PATH="../opencv_contrib/modules" \
    -DBUILD_DOCS=OFF \
    -DBUILD_TESTS=OFF \
    -DBUILD_EXAMPLES=OFF \
    -DBUILD_PERF_TESTS=OFF \
    -DBUILD_opencv_dnn=ON \
    -DTINYDNN_USE_NNPACK=OFF \
    -DTINYDNN_USE_TBB=ON \
    -DTINYDNN_USE_OMP=ON \
    -DENABLE_FAST_MATH=ON \
    -DWITH_OPENMP=ON \
    -DWITH_TBB=ON \
    -DMKL_WITH_TBB=ON \
    -DMKL_WITH_OPENMP=ON \
    -DCMAKE_CXX_COMPILER="/usr/bin/g++-5" \
    -DCMAKE_C_COMPILER="/usr/bin/gcc-5" \
    -DCUDA_HOST_COMPILER="/usr/bin/gcc-5" \
    -DCUDA_FAST_MATH=ON \
    -DCUDA_ARCH_BIN="5.2" \
    -DWITH_CUBLAS=ON \
    -DBUILD_opencv_python2=OFF \
    -DPYTHON_EXECUTABLE="/home/markus/anaconda3/bin/python3" \
    -DPYTHON_LIBRARY="/home/markus/anaconda3/lib/libpython3.6m.so" \
    -DPYTHON3_LIBRARY="/home/markus/anaconda3/lib/libpython3.6m.so" \
    -DPYTHON3_EXECUTABLE="/home/markus/anaconda3/bin/python3" \
    -DPYTHON3_INCLUDE_DIR="/home/markus/anaconda3/include/python3.6m" \
    -DPYTHON3_INCLUDE_DIR2="/home/markus/anaconda3/include" \
    -DPYTHON3_NUMPY_INCLUDE_DIRS="/home/markus/anaconda3/lib/python3.6/site-packages/numpy/core/include" \
    -DPYTHON3_INCLUDE_PATH="/home/markus/anaconda3/include/python3.6m" \
    -DPYTHON3_LIBRARIES="/home/markus/anaconda3/lib/libpython3.6m.so" \
    -DHDF5_C_LIBRARY_z="/home/markus/anaconda3/lib/libz.so" \
    ..
```

and then 

```bash
make -j10
make install
```

Targeting system Python version 2.7 and 3.5:

```bash
cmake \
    -DCMAKE_INSTALL_PREFIX="/usr/local" \
    -DOPENCV_EXTRA_MODULES_PATH="../opencv_contrib/modules" \
    -DBUILD_DOCS=OFF \
    -DBUILD_TESTS=OFF \
    -DBUILD_EXAMPLES=OFF \
    -DBUILD_PERF_TESTS=OFF \
    -DBUILD_opencv_dnn=OFF \
    -DENABLE_FAST_MATH=ON \
    -DWITH_OPENMP=ON \
    -DWITH_TBB=ON \
    -DMKL_WITH_TBB=ON \
    -DMKL_WITH_OPENMP=ON \
    -DCMAKE_CXX_COMPILER="/usr/bin/g++-5" \
    -DCMAKE_C_COMPILER="/usr/bin/gcc-5" \
    -DCUDA_HOST_COMPILER="/usr/bin/gcc-5" \
    -DCUDA_FAST_MATH=ON \
    -DCUDA_ARCH_BIN="5.2" \
    -DWITH_CUBLAS=ON \
    ..
```

and then 

```bash
make -j10
(sudo) make install
```
