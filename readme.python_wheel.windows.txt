
1. Prerequisites
================
- boost
- swig
- ore
- oresiwg

2. Build ORE
============

2.1 Use cmake to generate the project files

cd C:\erik\ORE\repos\oreplus\ore\build
"C:\Program Files\CMake\bin\cmake.exe" -G "Visual Studio 17 2022" -A x64 ..
-> C:\erik\ORE\repos\oreplus\ore\build\ORE.sln

2.1.1 Build using Visual Studio

C:\erik\ORE\repos\oreplus\ore\build\ORE.sln
-> C:\erik\ORE\repos\oreplus\ore\build\OREAnalytics\orea\Release\OREAnalytics-x64-mt.lib

2.1.2 Build using cmake

cd C:\erik\ORE\repos\oreplus\ore\build
"C:\Program Files\CMake\bin\cmake.exe" --build . --config Release
-> C:\erik\ORE\repos\oreplus\ore\build\OREAnalytics\orea\Release\OREAnalytics-x64-mt.lib

3. Build QuantLib
=================

4. Build QuantExt
=================

4.1 Use cmake to generate the project files

cd C:\erik\ORE\repos\oreswig.gitlab\buildQuantExt-SWIG
"C:\Program Files\CMake\bin\cmake.exe" -G "Visual Studio 17 2022" ^
-A x64 ^
-D SWIG_DIR=C:\erik\ORE\repos\swigwin-4.1.1\Lib ^
-D SWIG_EXECUTABLE=C:\erik\ORE\repos\swigwin-4.1.1\swig.exe ^
-D ORE:PATHNAME=C:\erik\ORE\repos\oreplus\ore ^
-D BOOST_ROOT=C:\erik\ORE\repos\boost_1_72_0 ^
-S C:\erik\ORE\repos\oreswig.gitlab\QuantExt-SWIG\Python
-> C:\erik\ORE\repos\oreswig.gitlab\buildQuantExt-SWIG\QuantExt-Python.sln

4.1.1 Build the pyd file using Visual Studio

C:\erik\ORE\repos\oreswig.gitlab\buildQuantExt-SWIG\QuantExt-Python.sln
-> C:\erik\ORE\repos\oreswig.gitlab\buildQuantExt-SWIG\Release\_QuantExt.pyd

4.1.2 Build the pyd file using cmake

cd C:\erik\ORE\repos\oreswig.gitlab\buildQuantExt-SWIG
"C:\Program Files\CMake\bin\cmake.exe" --build . --config Release
-> C:\erik\ORE\repos\oreswig.gitlab\buildQuantExt-SWIG\Release\_QuantExt.pyd

4.2 Build and use the wrapper

4.2.1 Build the wrapper

"C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Auxiliary\Build\vcvars64.bat"
cd C:\erik\ORE\repos\oreswig.gitlab\QuantExt-SWIG\Python
set BOOST_ROOT=C:\erik\ORE\repos\boost_1_72_0
set BOOST_LIB=C:\erik\ORE\repos\boost_1_72_0\lib\x64\lib
set ORE_DIR=C:\erik\ORE\repos\oreplus\ore
set PATH=%PATH%;C:\erik\ORE\repos\swigwin-4.1.1
python setup.py wrap
python setup.py build
-> C:\erik\ORE\repos\oreswig.gitlab\QuantExt-SWIG\Python\build\lib.win-amd64-cpython-310\QuantExt
python setup.py test (FAILS)

4.2.1 Use the wrapper

set PYTHONPATH=C:\erik\ORE\repos\oreswig.gitlab\QuantExt-SWIG\Python\build\lib.win-amd64-cpython-310
python C:\erik\ORE\repos\oreswig.gitlab\QuantExt-SWIG\Python\Examples\commodityforward.py

4.3.1 Build the wheel

cd C:\erik\ORE\repos\oreswig.gitlab\QuantExt-SWIG\Python
set BOOST_ROOT=C:\erik\ORE\repos\boost_1_72_0
set BOOST_LIB=C:\erik\ORE\repos\boost_1_72_0\lib\x64\lib
set ORE_DIR=C:\erik\ORE\repos\oreplus\ore
set PATH=%PATH%;C:\erik\ORE\repos\swigwin-4.1.1
set PATH=C:\Users\eric.ehlers\AppData\Local\Programs\Python\Python310\Scripts;%PATH%
#pip install build
python -m build --wheel
-> C:\erik\ORE\repos\oreswig.gitlab\QuantExt-SWIG\Python\dist\QuantExt_Python-1.8.7-cp310-cp310-win_amd64.whl

4.3.2 Use the wheel

python -m venv env1
.\env1\Scripts\activate.bat
pip install C:\erik\ORE\repos\oreswig.gitlab\QuantExt-SWIG\Python\dist\QuantExt_Python-1.8.7-cp310-cp310-win_amd64.whl
python C:\erik\ORE\repos\oreswig.gitlab\QuantExt-SWIG\Python\Examples\commodityforward.py

5. Build OREData-SWIG
=====================

WIP

6. Build OREPlus-SWIG
=====================

WIP

7. BUild OREAnalytics
=====================

7.1 Use cmake to generate the project files

cd C:\erik\ORE\repos\oreswig.gitlab\buildOREAnalytics-SWIG
"C:\Program Files\CMake\bin\cmake.exe" -G "Visual Studio 17 2022" ^
-A x64 ^
-D SWIG_DIR=C:\erik\ORE\repos\swigwin-4.1.1\Lib ^
-D SWIG_EXECUTABLE=C:\erik\ORE\repos\swigwin-4.1.1\swig.exe ^
-D ORE:PATHNAME=C:\erik\ORE\repos\oreplus\ore ^
-D BOOST_ROOT=C:\erik\ORE\repos\boost_1_72_0 ^
-S C:\erik\ORE\repos\oreswig.gitlab\OREAnalytics-SWIG\Python
-> C:\erik\ORE\repos\oreswig.gitlab\buildOREAnalytics-SWIG\OREAnalytics-Python.sln

7.1.1 Build the pyd file using Visual Studio

C:\erik\ORE\repos\oreswig.gitlab\buildOREAnalytics-SWIG\OREAnalytics-Python.sln
-> C:\erik\ORE\repos\oreswig.gitlab\buildOREAnalytics-SWIG\Release\_OREAnalytics.pyd

7.1.2 Build the pyd file using cmake

cd C:\erik\ORE\repos\oreswig.gitlab\buildOREAnalytics-SWIG
"C:\Program Files\CMake\bin\cmake.exe" --build . --config Release
-> C:\erik\ORE\repos\oreswig.gitlab\buildOREAnalytics-SWIG\Release\_OREAnalytics.pyd

7.2 Build and use the wrapper

7.2.1 Build the wrapper

"C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Auxiliary\Build\vcvars64.bat"
cd C:\erik\ORE\repos\oreswig.gitlab\OREAnalytics-SWIG\Python
set BOOST_ROOT=C:\erik\ORE\repos\boost_1_72_0
set BOOST_LIB=C:\erik\ORE\repos\boost_1_72_0\lib\x64\lib
set ORE_DIR=C:\erik\ORE\repos\oreplus\ore
set PATH=%PATH%;C:\erik\ORE\repos\swigwin-4.1.1
python setup.py wrap
python setup.py build

7.2.1 Use the wrapper

set PYTHONPATH=C:\erik\ORE\repos\oreswig.gitlab\OREAnalytics-SWIG\Python\build\lib.win-amd64-cpython-310
python C:\erik\ORE\repos\oreswig.gitlab\OREAnalytics-SWIG\Python\Examples\commodityforward.py

7.3.1 Build the wheel

cd C:\erik\ORE\repos\oreswig.gitlab\OREAnalytics-SWIG\Python
set BOOST_ROOT=C:\erik\ORE\repos\boost_1_72_0
set BOOST_LIB=C:\erik\ORE\repos\boost_1_72_0\lib\x64\lib
set ORE_DIR=C:\erik\ORE\repos\oreplus\ore
set PATH=%PATH%;C:\erik\ORE\repos\swigwin-4.1.1
set PATH=C:\Users\eric.ehlers\AppData\Local\Programs\Python\Python310\Scripts;%PATH%
python -m build --wheel
-> C:\erik\ORE\repos\oreswig.gitlab\OREAnalytics-SWIG\Python\dist\OREAnalytics_Python-1.8.3.2-cp310-cp310-win_amd64.whl

7.3.2 Use the wheel

python -m venv env1
.\env1\Scripts\activate.bat
pip install C:\erik\ORE\repos\oreswig.gitlab\OREAnalytics-SWIG\Python\dist\OREAnalytics_Python-1.8.3.2-cp310-cp310-win_amd64.whl
python C:\erik\ORE\repos\oreswig.gitlab\OREAnalytics-SWIG\Python\Examples\commodityforward.py

TODO
====

- C:\erik\ORE\repos\oreplus\ore\oreEverything.sln
  * QuantExtTestSuite not found

- "python setup.py test" fails

- many example py scripts do not work

