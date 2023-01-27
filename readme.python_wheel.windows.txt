
1. Introduction
================

This document provides a HOWTO for building and using a python wheel for OREAnalytics.

1.1 Other documentation

This document is fairly terse.  Refer to the resources below for more extensive information on the build and on the SWIG wrappers.

ore\userguide.pdf
ore-swig\README
ore-swig\README.md
ore-swig\OREAnalytics-SWIG\README

1.2 Prerequisites

- boost and swig: You need to either install the binaries,
  or install the source code and build yourself

- ore and oreswig: You need to install the source code.
  Instructions for building with cmake are provided below.

1.3 Environment variables

For purposes of this HOWTO, set the following environment variables to the paths where the above items live on your machine, e.g:

SET DEMO_SWIG_DIR=C:\erik\ORE\repos\swigwin-4.1.1
SET DEMO_BOOST_DIR=C:\erik\ORE\repos\boost_1_72_0
SET DEMO_ORE_DIR=C:\erik\ORE\repos\ore.eehlers
SET DEMO_ORE_SWIG_DIR=C:\erik\ORE\repos\oreswig.eehlers

2. Build ORE
============

2.1 Use cmake to generate the project files

cd %DEMO_ORE_DIR%
mkdir build
cd %DEMO_ORE_DIR%\build
"C:\Program Files\CMake\bin\cmake.exe" -G "Visual Studio 17 2022" -A x64 ..
-> %DEMO_ORE_DIR%\build\ORE.sln

2.1.1 EITHER Build using Visual Studio

%DEMO_ORE_DIR%\build\ORE.sln
-> %DEMO_ORE_DIR%\build\OREAnalytics\orea\Release\OREAnalytics-x64-mt.lib

2.1.2 OR Build using cmake

cd %DEMO_ORE_DIR%\build
"C:\Program Files\CMake\bin\cmake.exe" --build . --config Release
-> %DEMO_ORE_DIR%\build\OREAnalytics\orea\Release\OREAnalytics-x64-mt.lib

3. Build ORESWIG
================

3.1 Use cmake to generate the project files

cd %DEMO_ORE_SWIG_DIR%
mkdir buildOREAnalytics-SWIG
cd %DEMO_ORE_SWIG_DIR%\buildOREAnalytics-SWIG
"C:\Program Files\CMake\bin\cmake.exe" -G "Visual Studio 17 2022" ^
-A x64 ^
-D SWIG_DIR=%DEMO_SWIG_DIR%\Lib ^
-D SWIG_EXECUTABLE=%DEMO_SWIG_DIR%\swig.exe ^
-D ORE:PATHNAME=%DEMO_ORE_DIR% ^
-D BOOST_ROOT=%DEMO_BOOST_DIR% ^
-S %DEMO_ORE_SWIG_DIR%\OREAnalytics-SWIG\Python
-> %DEMO_ORE_SWIG_DIR%\buildOREAnalytics-SWIG\OREAnalytics-Python.sln

3.1.1 EITHER Build the pyd file using Visual Studio

%DEMO_ORE_SWIG_DIR%\buildOREAnalytics-SWIG\OREAnalytics-Python.sln
-> %DEMO_ORE_SWIG_DIR%\buildOREAnalytics-SWIG\Release\_OREAnalytics.pyd

3.1.2 OR Build the pyd file using cmake

cd %DEMO_ORE_SWIG_DIR%\buildOREAnalytics-SWIG
"C:\Program Files\CMake\bin\cmake.exe" --build . --config Release
-> %DEMO_ORE_SWIG_DIR%\buildOREAnalytics-SWIG\Release\_OREAnalytics.pyd

3.2 Build and use the wrapper

3.2.1 Build the wrapper

"C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Auxiliary\Build\vcvars64.bat"
cd %DEMO_ORE_SWIG_DIR%\OREAnalytics-SWIG\Python
set BOOST_ROOT=%DEMO_BOOST_DIR%
set BOOST_LIB=%DEMO_BOOST_DIR%\lib\x64\lib
set ORE_DIR=%DEMO_ORE_DIR%
set PATH=%PATH%;%DEMO_SWIG_DIR%
python setup.py wrap
python setup.py build

3.2.1 Use the wrapper

set PYTHONPATH=%DEMO_ORE_SWIG_DIR%\OREAnalytics-SWIG\Python\build\lib.win-amd64-cpython-310
cd %DEMO_ORE_SWIG_DIR%\OREAnalytics-SWIG\Python\Examples
python commodityforward.py

3.3.1 Build the wheel

cd %DEMO_ORE_SWIG_DIR%\OREAnalytics-SWIG\Python
set BOOST_ROOT=%DEMO_BOOST_DIR%
set BOOST_LIB=%DEMO_BOOST_DIR%\lib\x64\lib
set ORE_DIR=%DEMO_ORE_DIR%
set PATH=%PATH%;%DEMO_SWIG_DIR%
set PATH=C:\Users\eric.ehlers\AppData\Local\Programs\Python\Python310\Scripts;%PATH%
python -m build --wheel
-> %DEMO_ORE_SWIG_DIR%\OREAnalytics-SWIG\Python\dist\OREAnalytics_Python-1.8.3.2-cp310-cp310-win_amd64.whl

3.3.2 Use the wheel

python -m venv env1
.\env1\Scripts\activate.bat
pip install %DEMO_ORE_SWIG_DIR%\OREAnalytics-SWIG\Python\dist\OREAnalytics_Python-1.8.3.2-cp310-cp310-win_amd64.whl
python %DEMO_ORE_SWIG_DIR%\OREAnalytics-SWIG\Python\Examples\commodityforward.py

===============================================================================
NB: The build above for ORE includes the build of QuantLib and QuantExt.  Below
are instructions for building QuantLib and QuantExt standalone which may be
helpful for troubleshooting or other purposes.
===============================================================================

4. Build QuantLib
=================

4.1 Build Quantlib

# TODO: HOWTO for building QL w/native tools
-> %DEMO_ORE_DIR%\QuantLib\lib\QuantLib-x64-mt.lib

4.2 Build Quantlib-SWIG (wrapper and wheel)

cd %DEMO_ORE_SWIG_DIR%\QuantLib-SWIG\Python
set PATH=%PATH%;%DEMO_SWIG_DIR%
python setup.py wrap
set QL_DIR=%DEMO_ORE_DIR%\QuantLib
set INCLUDE=%DEMO_BOOST_DIR%
set LIB=%DEMO_BOOST_DIR%\lib\x64\lib
python setup.py build
python setup.py test
python -m build --wheel

4.3 Use the wrapper

cd %DEMO_ORE_SWIG_DIR%\QuantLib-SWIG\Python\examples
set PYTHONPATH=%DEMO_ORE_SWIG_DIR%\QuantLib-SWIG\Python\build\lib.win-amd64-cpython-310
python swap.py

4.4 Use the wheel

cd %DEMO_ORE_SWIG_DIR%\QuantLib-SWIG\Python\examples
python -m venv env1
.\env1\Scripts\activate.bat
pip install %DEMO_ORE_SWIG_DIR%\QuantLib-SWIG\Python\dist\QuantLib-1.28-cp310-cp310-win_amd64.whl
python swap.py
deactivate
rmdir /s /q env1

5. Build QuantExt
=================

5.1 Use cmake to generate the project files

cd %DEMO_ORE_SWIG_DIR%\buildQuantExt-SWIG
"C:\Program Files\CMake\bin\cmake.exe" -G "Visual Studio 17 2022" ^
-A x64 ^
-D SWIG_DIR=%DEMO_SWIG_DIR%\Lib ^
-D SWIG_EXECUTABLE=%DEMO_SWIG_DIR%\swig.exe ^
-D ORE:PATHNAME=%DEMO_ORE_DIR% ^
-D BOOST_ROOT=%DEMO_BOOST_DIR% ^
-S %DEMO_ORE_SWIG_DIR%\QuantExt-SWIG\Python
-> %DEMO_ORE_SWIG_DIR%\buildQuantExt-SWIG\QuantExt-Python.sln

5.1.1 EITHER Build the pyd file using Visual Studio

%DEMO_ORE_SWIG_DIR%\buildQuantExt-SWIG\QuantExt-Python.sln
-> %DEMO_ORE_SWIG_DIR%\buildQuantExt-SWIG\Release\_QuantExt.pyd

5.1.2 OR Build the pyd file using cmake

cd %DEMO_ORE_SWIG_DIR%\buildQuantExt-SWIG
"C:\Program Files\CMake\bin\cmake.exe" --build . --config Release
-> %DEMO_ORE_SWIG_DIR%\buildQuantExt-SWIG\Release\_QuantExt.pyd

5.2 Build and use the wrapper

5.2.1 Build the wrapper

"C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Auxiliary\Build\vcvars64.bat"
cd %DEMO_ORE_SWIG_DIR%\QuantExt-SWIG\Python
set BOOST_ROOT=%DEMO_BOOST_DIR%
set BOOST_LIB=%DEMO_BOOST_DIR%\lib\x64\lib
set ORE_DIR=%DEMO_ORE_DIR%
set PATH=%PATH%;%DEMO_SWIG_DIR%
python setup.py wrap
python setup.py build
-> %DEMO_ORE_SWIG_DIR%\QuantExt-SWIG\Python\build\lib.win-amd64-cpython-310\QuantExt
python setup.py test (FAILS)

5.2.1 Use the wrapper

set PYTHONPATH=%DEMO_ORE_SWIG_DIR%\QuantExt-SWIG\Python\build\lib.win-amd64-cpython-310
python %DEMO_ORE_SWIG_DIR%\QuantExt-SWIG\Python\Examples\commodityforward.py

5.3.1 Build the wheel

cd %DEMO_ORE_SWIG_DIR%\QuantExt-SWIG\Python
set BOOST_ROOT=%DEMO_BOOST_DIR%
set BOOST_LIB=%DEMO_BOOST_DIR%\lib\x64\lib
set ORE_DIR=%DEMO_ORE_DIR%
set PATH=%PATH%;%DEMO_SWIG_DIR%
set PATH=C:\Users\eric.ehlers\AppData\Local\Programs\Python\Python310\Scripts;%PATH%
#pip install build
python -m build --wheel
-> %DEMO_ORE_SWIG_DIR%\QuantExt-SWIG\Python\dist\QuantExt_Python-1.8.7-cp310-cp310-win_amd64.whl

5.3.2 Use the wheel

python -m venv env1
.\env1\Scripts\activate.bat
pip install %DEMO_ORE_SWIG_DIR%\QuantExt-SWIG\Python\dist\QuantExt_Python-1.8.7-cp310-cp310-win_amd64.whl
python %DEMO_ORE_SWIG_DIR%\QuantExt-SWIG\Python\Examples\commodityforward.py

TODO
====

- %DEMO_ORE_DIR%\oreEverything.sln
  * QuantExtTestSuite not found

- "python setup.py test" fails

- many example py scripts do not work
