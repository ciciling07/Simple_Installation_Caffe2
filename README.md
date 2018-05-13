# Simple_Installation_Caffe2
This Repo records how to install Caffe2 (in Pytorch Repo) with python virtualenv from source code.

System environment
Ubuntu 16.04
40 CPU

# Step 1 some system preparation!
if you have sudo, it's the best. But if you don't you can download the source code and compile from source. some of lib below are not required because third-party of caffe2 lib already have stuff.
But opencv, lmdb, openmpi, gflags, glog are required. You can also compile python from source if you don't have sudo. I recommend to use python3.

sudo apt-get update
sudo apt-get install -y --no-install-recommends \
build-essential \
cmake \
git \
libgoogle-glog-dev \
libgtest-dev \
libiomp-dev \
liblmdb-dev \
libopencv-dev \
libopenmpi-dev \
libsnappy-dev \
libprotobuf-dev \
openmpi-bin \
openmpi-doc \
protobuf-compiler \
python-dev \
python-pip
sudo apt-get install -y --no-install-recommends libgflags-dev
sudo pip install virtualenv

# Step 2 virtualenv!
we create a virutalenv python in our home dir. this will be easier for reinstallation if you fail to install caffe2 at first time.
I recommend you to know which python you are using. <which python> <which pip>.

cd ~
virtualenv -p /usr/bib/python3(or your python bin) py3caffe2 
source /home/yuxi/py3caffe2/bin/activate
pip install future numpy protobuf

# Step 3 download pytorch
caffe2 is in pytorch hub.

git clone --recursivehttps://github.com/pytorch/pytorch.git&& cd pytorch
git submodule update --init

[important] Change CMakelist.txt in pytorch,add 
set(PYTHON_INCLUDE_DIR "/home/yuxi/py3caffe2/include/python3.5m")
set(PYTHON_LIBRARY "/home/yuxi/py3caffe2/lib")
to your CMakelist.txt.
So your CMakelist.txt will be like:

 project(Caffe2 CXX C)
 set(CAFFE2_VERSION_MAJOR 0)
 set(CAFFE2_VERSION_MINOR 8)
 set(CAFFE2_VERSION_PATCH 2)
 set(CAFFE2_VERSION "${CAFFE2_VERSION_MAJOR}.${CAFFE2_VERSION_MINOR}.${CAFFE2_VERSION_PATCH}")
 set(CAFFE2_CMAKE_BUILDING_WITH_MAIN_REPO ON)
 set(PYTHON_INCLUDE_DIR "/home/yuxi/py3caffe2/include/python3.5m")
 set(PYTHON_LIBRARY "/home/yuxi/py3caffe2/lib")
 include(CMakeDependentOption)
 
 
# make a build dir and cmake and build
mkdir build && cd build
cmake ..
make -j40 install DESTDIR=./INSTALL


# after compilation and modify your bashrc

copy your compilation to python lib
cd INSTALL/usr/local && cp -rf * ~/py3caffe2/
modify the bashrc for C++ use.
export PATH=$PATH:/home/yuxi/py3caffe2/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/yuxi/py3caffe2/lib
source /home/yuxi/py3caffe2/bin/activate





