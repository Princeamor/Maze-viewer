# Maze-viewer
Fork of Firestorm viewer with improvements for Opensim
# Required Software for Building Aperture Viewer on Windows 11

- **Host and hardware**: Windows 11 64-bit on an AVX-capable CPU (per release build note in Aperture Viewer README).
- **Version control**: Git (64-bit). Needed to clone Aperture-Viewer and the aperture-build-variables repo.
- **Python**: Python 3.10+ (64-bit) with pip. Install the build helpers from [Aperture-Viewer/requirements.txt](Aperture/Aperture-Viewer/requirements.txt):
  - `pip install -r requirements.txt` (packages: autobuild, llbase, llsd).
- **Autobuild variables**: Clone [aperture-build-variables](Aperture/aperture-build-variables/README.md) and set `AUTOBUILD_VARIABLES_FILE` to the `variables` file inside that clone (required by the build system).
- **Editor/IDE**: Visual Studio Code with the C/C++ and CMake Tools extensions. VS Code orchestrates CMake configure/build/debug by calling the Visual Studio toolchain below (msvc, Windows SDK, and the CMake/Ninja tools that ship with the VS workload).
- **Compiler toolchain**: Visual Studio 2022 (v17) with the "Desktop development with C++" workload, including:
  - MSVC v143 toolset (x64) and C++ CMake tools for Windows (VS Code will invoke these for builds).
  - Windows 10 or 11 SDK (10.0.22000+ recommended; VS Code consumes this via the MSVC environment).
  - ATL and MFC support (used by Firestorm/Aperture builds).
- **CMake**: CMake 3.24+ (install separately if not added via Visual Studio components; VS Code will use the detected CMake installation).
- **Installer tooling**: NSIS 3.x or newer (required; noted in installer_template.nsi for Unicode support).
- **Automatically downloaded third-party libraries**: Autobuild pulls all viewer dependencies defined in [autobuild.xml](Aperture/Aperture-Viewer/autobuild.xml) (SDL2, gstreamer, breakpad, glod, discord-rpc, webrtc, etc.), so no manual installs are needed beyond the tools above.

## Build Steps (Windows 11, VS Code + VS 2022)

1) Download the zip file here at https://maze.bz/src.zip. If you unzip it to your C: drive you will find the powershell should work right off the bat long as you install the other neccessary dependencies listed above.
DO NOT move the files around, or you may not be able to build.
2) If you use copilot in VS Code you can ask it just to run the cleanautobuild.ps1 and it will most likely do it for you. You have to have all teh dependcies for VS 2022 installed C++etc... or it will fail.

3) Set environment for autobuild variables (PowerShell example):
  - `$env:AUTOBUILD_VARIABLES_FILE="C:\path\to\aperture-build-variables\variables"`
  - Consider setting this as a permanent user/system env var so VS Code inherits it.

3) Install Python deps (from repo root):
  - `pip install -r requirements.txt` (installs autobuild, llbase, llsd)

4) Launch VS Code in the Aperture-Viewer repo: `code .`
  - Ensure the C/C++ and CMake Tools extensions are installed/enabled.

5) Configure the kit in VS Code (Command Palette → "CMake: Select a Kit"):
  - Pick the MSVC 64-bit kit from VS 2022 (v143) so VS Code uses that toolchain and Windows SDK.

6) Configure and build:
  - Command Palette → "CMake: Configure" (CMake Tools will invoke the MSVC environment and detect the SDK).
  - Command Palette → "CMake: Build" (or use the status bar Build button). This runs the generated build with MSVC/Ninja.

7) Run packaging/installer (if desired):
  - From a VS Developer PowerShell with env set, run the viewer packaging target or `autobuild build --config Release`. NSIS 3+ must be on PATH for the installer stage.

Notes:
  - If CMake cannot find the Windows SDK or MSVC, launch VS Code from a "Developer Command Prompt for VS 2022" or ensure VS Build Tools are fully installed.
  - Autobuild will download third-party libs on first configure/build; allow time and ensure network access.

DO ENJOY!!
