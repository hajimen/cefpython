# Build instructions

Table of contents:
* [Preface](#preface)
* [Quick build instructions for Windows](#quick-build-instructions-for-windows)
* [Requirements](#requirements)


## Preface

These instructions are for cefpython3-v66.1-for-python-3.12-unofficial.

Before you can build CEF Python or CEF you must satisfy
[requirements](#requirements) listed on this page.


## Quick build instructions for Windows

Complete steps for building cefpython3-v66.1-for-python-3.12-unofficial.

1) Tested and works fine on Windows 11 x86-64 and Visual Studio 2022.

2) Open Windows Terminal and open "Developer PowerShell for VS2022" from "\/" button.

3) Setup venv.

```
python -m venv venv_cefpython
venv_cefpython/Scripts/Activate.ps1
```

4) Clone cefpython3-v66.1-for-python-3.12-unofficial and prepare `build` directory.

```
git clone -b cefpython3-v66.1-for-python-3.12-unofficial https://github.com/hajimen/cefpython.git
cd cefpython
mkdir build
cd build
```

5) Install python dependencies:

```
pip install --upgrade -r ../tools/requirements.txt
```

6) Download Windows binaries and sources from
   [Spotify CDN](https://cef-builds.spotifycdn.com/index.html).
   The version of the binaries must match exactly the CEF version from
   the "cefpython/src/version/cef_version_win.h" file
   (the CEF_VERSION constant). For cefpython3-v66.1-for-python-3.12-unofficial,
   [this](https://cef-builds.spotifycdn.com/cef_binary_3.3359.1774.gd49d25f_windows64.tar.bz2).

7) Extract the archive in the "build/" directory. Rename the directory to
   `cef66_3.3359.1774.gd49d25f_win64` or something like it.

8) Build libcef_dll_wrapper of CEF.

```
cd cef66_3.3359.1774.gd49d25f_win64
mkdir build
cd build
cmake -G "Visual Studio 17 2022" ..
```

Open `cef.sln` by VS2022, choose `libcef_dll_wrapper`, Alt+Enter, set `Configuration` to `Release`,
choose `C/C++ -> Code Generation`, `Runtime Library`, and change it to `Multi-threaded DLL (/MD)`.
Save the change and close VS2022.

```
msbuild cef.sln /target:libcef_dll_wrapper /p:Configuration=Release
cd ../..
```

9) Build cefpython and run examples (66.1 is version number of cefpython):

```
python ../tools/build.py 66.1
```

10) To build an whl (66.1 is version number of cefpython):

```
python ..\tools\build_distrib.py 66.1 --no-automate
```

Rebuild consumes much time. To avoid redundant rebuild, add `--no-rebuild`.

## Requirements

Below are platform specific requirements. Do these first before
following instructions in the "All platforms" section that lists
requirements common for all platforms.

### Windows

* Visual Studio 2022

* Python 3.12, 3.11 and 3.10 x86-64 distributed from [python.org](https://www.python.org/downloads/).

* Windows Terminal
