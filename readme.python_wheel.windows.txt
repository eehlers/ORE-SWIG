
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

- python - This HOWTO will require you to have the following tools up to date:

python -m ensurepip
pip install build

- boost and swig: You need to either install the binaries,
  or install the source code and build yourself

- ore and oreswig: You need to install the source code.
  Instructions for building with cmake are provided below.

1.3 Environment variables

For purposes of this HOWTO, set the following environment variables to the paths where the above items live on your machine, e.g:

SET DEMO_BOOST_ROOT=C:\erik\ORE\repos\boost\boost_1_81_0
SET DEMO_BOOST_LIB=C:\erik\ORE\repos\boost\boost_1_81_0\lib\x64\lib
SET DEMO_SWIG_DIR=C:\erik\ORE\repos\swigwin-4.1.1
SET DEMO_ORE_DIR=C:\erik\ORE\repos\ore.eehlers
SET DEMO_ORE_SWIG_DIR=C:\erik\ORE\repos\oreswig.eehlers

2. Build ORE
============

2.1 Use cmake to generate the project files

cd %DEMO_ORE_DIR%
mkdir build
cd %DEMO_ORE_DIR%\build
#set BOOST_ROOT=%DEMO_BOOST_ROOT%
set BOOST_INCLUDEDIR=%DEMO_BOOST_ROOT%
set BOOST_LIBRARYDIR=%DEMO_BOOST_LIB%
"C:\Program Files\CMake\bin\cmake.exe" -DMSVC_LINK_DYNAMIC_RUNTIME=OFF -DBoost_NO_WARN_NEW_VERSIONS=1 -Wno-dev -G "Visual Studio 17 2022" -A x64 ..
-> %DEMO_ORE_DIR%\build\ORE.sln

2.1.1 EITHER Build using Visual Studio

%DEMO_ORE_DIR%\build\ORE.sln
-> %DEMO_ORE_DIR%\build\OREAnalytics\orea\Release\OREAnalytics-x64-mt.lib

2.1.2 OR Build using cmake

cd %DEMO_ORE_DIR%\build
"C:\Program Files\CMake\bin\cmake.exe" --build . --config Release
-> %DEMO_ORE_DIR%\build\OREAnalytics\orea\Release\OREAnalytics-x64-mt.lib

3. Build ORE-SWIG
=================

3.1 Build ORE-SWIG (wrapper and wheel)

===============================================================================
NB: The step below builds the wrapper using the static runtime, so that end users have no dependencies.
In order to do that:
1) You need to set the variable ORE_STATIC_RUNTIME as noted below
2) You need a special copy of setup.py which has been amended to read that environment variable.
   You can get the file here:

https://github.com/eehlers/ORE-SWIG/blob/master/OREAnalytics-SWIG/Python/setup.py

   And you need to copy it to this location:

%DEMO_ORE_SWIG_DIR%\OREAnalytics-SWIG\Python\setup.py
===============================================================================

cd %DEMO_ORE_SWIG_DIR%\OREAnalytics-SWIG\Python
set BOOST_ROOT=%DEMO_BOOST_ROOT%
set BOOST_LIB=%DEMO_BOOST_LIB%
set ORE_DIR=%DEMO_ORE_DIR%
set PATH=%PATH%;%DEMO_SWIG_DIR%
set ORE_STATIC_RUNTIME=1
python setup.py wrap
python setup.py build
python setup.py test
python -m build --wheel

3.2 Use the wrapper

cd %DEMO_ORE_SWIG_DIR%\OREAnalytics-SWIG\Python\Examples
set PYTHONPATH=%DEMO_ORE_SWIG_DIR%\OREAnalytics-SWIG\Python\build\lib.win-amd64-cpython-310
python commodityforward.py

3.3 Use the wheel

cd %DEMO_ORE_SWIG_DIR%\OREAnalytics-SWIG\Python\Examples
python -m venv env1
.\env1\Scripts\activate.bat
pip install %DEMO_ORE_SWIG_DIR%\OREAnalytics-SWIG\Python\dist\OREAnalytics_Python-1.8.3.2-cp310-cp310-win_amd64.whl
python commodityforward.py
deactivate
rmdir /s /q env1

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
set QL_DIR=%DEMO_ORE_DIR%\QuantLib
set INCLUDE=%DEMO_BOOST_ROOT%
set LIB=%DEMO_BOOST_LIB%
python setup.py wrap
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

5.1 Build QuantExt

# TODO: HOWTO for building QLE w/native tools
-> %DEMO_ORE_DIR%\QuantExt\lib\QuantExt-x64-mt.lib

5.2 Build QuantExt-SWIG (wrapper and wheel)

"C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Auxiliary\Build\vcvars64.bat"
cd %DEMO_ORE_SWIG_DIR%\QuantExt-SWIG\Python
set BOOST_ROOT=%DEMO_BOOST_ROOT%
set BOOST_LIB=%DEMO_BOOST_LIB%
set ORE_DIR=%DEMO_ORE_DIR%
set PATH=%PATH%;%DEMO_SWIG_DIR%
python setup.py wrap
python setup.py build
python setup.py test (FAILS)
python -m build --wheel

5.3 Use the wrapper

cd %DEMO_ORE_SWIG_DIR%\QuantExt-SWIG\Python\Examples
set PYTHONPATH=%DEMO_ORE_SWIG_DIR%\QuantExt-SWIG\Python\build\lib.win-amd64-cpython-310
python commodityforward.py

5.4 Use the wheel

cd %DEMO_ORE_SWIG_DIR%\QuantExt-SWIG\Python\Examples
python -m venv env1
.\env1\Scripts\activate.bat
pip install %DEMO_ORE_SWIG_DIR%\QuantExt-SWIG\Python\dist\QuantExt_Python-1.8.7-cp310-cp310-win_amd64.whl
python commodityforward.py
deactivate
rmdir /s /q env1

TODO
====

- %DEMO_ORE_DIR%\oreEverything.sln
  * QuantExtTestSuite not found

- "python setup.py test" fails

- many example py scripts do not work

