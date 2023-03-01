
1. Introduction
================

This document provides a HOWTO for building and using a python wheel for OREAnalytics.

1.1 Other documentation

This document is fairly terse.  Refer to the resources below for more extensive information on the build and on the SWIG wrappers.

ore/userguide.pdf
oreswig/README
oreswig/README.md
oreswig/OREAnalytics-SWIG/README

1.2 Prerequisites

- python - This HOWTO will require you to have the following tools up to date:

sudo apt install python3-pip
sudo apt install python3-venv
pip install build

- boost and swig: You need to either install the binaries,
  or install the source code and build yourself

- ore and oreswig: You need to install the source code.
  Instructions for building with cmake are provided below.

1.3 Environment variables

For purposes of this HOWTO, set the following environment variables to the paths where the above items live on your machine, e.g:

DEMO_BOOST_DIR=/home/erik/quaternion/boost_1_81_0
DEMO_ORE_DIR=/home/erik/quaternion/ore.eehlers
DEMO_ORE_SWIG_DIR=/home/erik/quaternion/oreswig.eehlers

2. Build ORE
============

2.1 Use cmake to generate the project files

cd $DEMO_ORE_DIR
mkdir build
cd $DEMO_ORE_DIR/build
cmake -DCMAKE_POSITION_INDEPENDENT_CODE=ON -DORE_BUILD_DOC=OFF -DORE_BUILD_EXAMPLES=OFF -DORE_BUILD_TESTS=OFF -DORE_BUILD_APP=OFF \
-DBoost_NO_WARN_NEW_VERSIONS=1 -DBoost_NO_SYSTEM_PATHS=1 -DBOOST_ROOT=$DEMO_BOOST_DIR ..
-> $DEMO_ORE_DIR/build/Makefile
BOOST_BIND_GLOBAL_PLACEHOLDERS

2.1.1 EITHER Build using make

cd $DEMO_ORE_DIR/build
make
-> $DEMO_ORE_DIR/build/OREAnalytics/orea/libOREAnalytics.so

2.1.2 OR Build using cmake

cd $DEMO_ORE_DIR/build
cmake --build .
-> $DEMO_ORE_DIR/build/OREAnalytics/orea/libOREAnalytics.so

3. Build ORE-SWIG
=====================

3.1 Build ORE-SWIG (wrapper and wheel)

cd $DEMO_ORE_SWIG_DIR/OREAnalytics-SWIG/Python
#export BOOST_ROOT=$DEMO_BOOST_DIR
#export BOOST_LIB=$DEMO_BOOST_DIR/stage/lib
export ORE=$DEMO_ORE_DIR
export BOOST=$DEMO_BOOST_DIR
python3 setup.py wrap
python3 setup.py build # swap space
-> $DEMO_ORE_SWIG_DIR/OREAnalytics-SWIG/Python/build/lib.linux-x86_64-3.10/OREAnalytics/_OREAnalytics.cpython-310-x86_64-linux-gnu.so
python3 -m build --wheel
-> $DEMO_ORE_SWIG_DIR\OREAnalytics-SWIG\Python\dist\OREAnalytics_Python-1.8.3.2-cp310-cp310-win_amd64.whl

3.2 Use the wrapper

cd $DEMO_ORE_SWIG_DIR/OREAnalytics-SWIG/Python/Examples
export PYTHONPATH=$DEMO_ORE_SWIG_DIR/OREAnalytics-SWIG/Python/build/lib.linux-x86_64-3.10/OREAnalytics
export LD_LIBRARY_PATH=$DEMO_ORE_DIR/build/OREAnalytics/orea:$DEMO_ORE_DIR/build/OREData/ored:$DEMO_ORE_DIR/build/QuantExt/qle:$DEMO_ORE_DIR/build/QuantLib/ql:/home/erik/quaternion/boost_1_81_0/stage/lib
python3 swap.py -> segmentation fault
python3 commodityforward.py

3.3 Use the wheel

cd $DEMO_ORE_SWIG_DIR/OREAnalytics-SWIG/Python/Examples
python3 -m venv env1
. ./env1/bin/activate
pip install $DEMO_ORE_SWIG_DIR/OREAnalytics-SWIG/Python/dist/OREAnalytics_Python-1.8.3.2-cp310-cp310-linux_x86_64.whl
python3 commodityforward.py
deactivate
rm -rf env1

===============================================================================
NB: The build above for ORE includes the build of QuantLib and QuantExt.  Below
are instructions for building QuantLib and QuantExt standalone which may be
helpful for troubleshooting or other purposes.
===============================================================================

4. Build QuantLib
=================

4.1 Build Quantlib

cd $DEMO_ORE_DIR/QuantLib
./autogen.sh
./configure --with-boost-include=$DEMO_BOOST_DIR --with-boost-lib=$DEMO_BOOST_DIR/stage/lib
make

4.2 Build Quantlib-SWIG (wrapper and wheel)

# FIXME the commands below, are they redundant, and replicated by the wrap command that follows?
cd $DEMO_ORE_SWIG_DIR/QuantLib-SWIG
./autogen.sh
./configure
make -C Python

cd $DEMO_ORE_SWIG_DIR/QuantLib-SWIG/Python
export PATH=$PATH:$DEMO_ORE_DIR/QuantLib
export CXXFLAGS=-I$DEMO_ORE_DIR/QuantLib
export LDFLAGS=-L$DEMO_ORE_DIR/QuantLib/ql/.libs
python3 setup.py wrap
python3 setup.py build
python3 -m build --wheel

4.3 Use the wrapper

cd $DEMO_ORE_SWIG_DIR/QuantLib-SWIG/Python/examples
export PYTHONPATH=$DEMO_ORE_SWIG_DIR/QuantLib-SWIG/Python/build/lib.linux-x86_64-3.10/QuantLib
export LD_LIBRARY_PATH=$DEMO_ORE_DIR/QuantLib/ql/.libs:/home/erik/quaternion/boost_1_81_0/stage/lib
python3 swap.py

4.4 Use the wheel

WIP

5. Build QuantExt
=================

5.1 Build QuantExt

# TODO: HOWTO for building QLE w/native tools
-> %DEMO_ORE_DIR%\QuantExt\lib\QuantExt-x64-mt.lib

5.2 Build QuantExt-SWIG (wrapper and wheel)

cd $DEMO_ORE_SWIG_DIR/QuantExt-SWIG/Python
#export BOOST_ROOT=$DEMO_BOOST_DIR
#export BOOST_LIB=$DEMO_BOOST_DIR/stage/lib
#export ORE_DIR=$DEMO_ORE_DIR
export ORE=$DEMO_ORE_DIR
python3 setup.py wrap
python3 setup.py build -> FAILS **************************************
-> $DEMO_ORE_SWIG_DIR\QuantExt-SWIG\Python\build\lib.win-amd64-cpython-310\QuantExt
python setup.py test (FAILS)
python -m build --wheel
-> $DEMO_ORE_SWIG_DIR\QuantExt-SWIG\Python\dist\QuantExt_Python-1.8.7-cp310-cp310-win_amd64.whl

5.3 Use the wrapper

set PYTHONPATH=$DEMO_ORE_SWIG_DIR\QuantExt-SWIG\Python\build\lib.win-amd64-cpython-310
python $DEMO_ORE_SWIG_DIR\QuantExt-SWIG\Python\Examples\commodityforward.py

5.4 Use the wheel

cd $DEMO_ORE_SWIG_DIR\QuantExt-SWIG\Python\Examples
python -m venv env1
.\env1\Scripts\activate.bat
pip install $DEMO_ORE_SWIG_DIR\QuantExt-SWIG\Python\dist\QuantExt_Python-1.8.7-cp310-cp310-win_amd64.whl
python commodityforward.py
deactivate
rm -rf env1

TODO
====

- $DEMO_ORE_DIR\oreEverything.sln
  * QuantExtTestSuite not found

- "python setup.py test" fails

- many example py scripts do not work

