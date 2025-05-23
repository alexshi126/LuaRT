<div align="center">

![LuaRT][title] 

[![Lua VM 5.4.7](https://badgen.net/badge/Lua%20VM/5.4/yellow)](https://www.lua.org/)
![Windows](https://badgen.net/badge/Windows/Windows%208.1+/blue?icon=windows)
[![LuaRT license](https://badgen.net/badge/License/MIT/green)](#license)
[![Codacy Badge](https://app.codacy.com/project/badge/Grade/af54881b3d764f5ea210a5419fb96086)](https://www.codacy.com/gh/samyeyo/LuaRT/dashboard?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=samyeyo/LuaRT&amp;utm_campaign=Badge_Grade)  
[![Twitter Follow](https://img.shields.io/twitter/follow/__LuaRT__?style=social)](https://www.twitter.com/__LuaRT__)

Lua multipurpose programming framework to develop Windows applications

![Banner][banner] 

[Features](#small_blue_diamondfeatures) |
[Installation](#small_blue_diamondinstallation) |
[Documentation](https://www.luart.org/doc/index.html) |
[Supporting](#small_blue_diamondsupporting) |
[Links](#small_blue_diamondlinks) |
[License](#small_blue_diamondlicense)

</div>
   
## :small_blue_diamond:Features

#### Lua for Windows, with batteries included
- Multipurpose programming framework with optimized Lua runtime library for x86 and x64 Windows.
- Build Windows desktop or console applications with Lua
- Lightweight with no other dependencies
- Develop in Lua, C programming knowledge is not needed
- Batteries included : UTF8 strings, sockets, GUI, compression, audio, graphics, C FFI...
- LuaRT runs on Windows 8.1, Windows 10 and Windows 11.

#### Complete development environment 
- rtc: a Lua script to executable compiler with static compilation and binary modules support
- QuickRT: a powerful Lua REPL
- LuaRT Studio: a Lua/LuaRT IDE for Windows to develop and debug desktop/console applications
  
## :small_blue_diamond:Installation

#### Method 1 : Release package :package:

The preferred way to install LuaRT is to download the latest release package available on GitHub, and run the setup executable.
It will install the LuaRT binaries, create the Windows Start menu shortcuts for the IDE and REPL, and update the PATH system variable. 
It's the easiest and fastest way to start developing with LuaRT.

> If you have already installed LuaRT and want to update to latest release version, you can run the `LuaRT Update` tool from the Windows Start menu.

#### Method 2 : Building from sources (Visual C++) :gear:

All you need to build LuaRT from sources is a valid installation of Visual C++ compiler.
Before proceeding, be sure to have a valid Visual Studio (Build Tools, Community, Professional or Enterprise) installation. Release packages are built using latest Visual Studio Enterprise version.

First open a console using `x86 Native Tools Command Prompt` (for LuaRT x86) or `x64 Native Tools Command Prompt` (for LuaRT x64) shortcuts in your Windows Start menu.
Then clone the LuaRT repository (or manualy download the repository) :
```
git clone https://github.com/samyeyo/LuaRT.git
```

Go to the ```\src``` directory and type ```nmake```:

- `nmake` : Build LuaRT library and executable 
- `nmake debug`: Build debug versions of LuaRT library and executables
- `nmake clean` : Clean all the generated binaries

If everything went right, the `\bin` folder will contain the LuaRT toolchain :
- ```lua54.dll``` : the LuaRT shared library, ABI compatible with the standard lua54.dll
- ```luart.exe``` : the LuaRT console interpreter
- ```wluart.exe```: the desktop LuaRT interpreter
- ```luart-static.exe```: the LuaRT console interpreter, without ```lua54.dll``` dependency
- ```wluart-static.exe```: the desktop LuaRT interpreter, without ```lua54.dll``` dependency
- ```rtc.exe``` : the Lua script to executable compiler
- ```wrtc.exe``` : the GUI front-end for rtc

You must now add the ```\bin\``` directory to the system PATH (set it accordingly to your LuaRT path), for example :

```
SET PATH=%PATH%;"C:\LuaRT\bin"
```

## :small_blue_diamond:Usage

```
luart.exe [-e "statement"] [filename] [arg1 arg2...]
wluart.exe [-e "statement"] [filename] [arg1 arg2...]

-e "statement"
Executes the Lua statement in double quotes and exits.

filename [arg1 arg2...]
Loads and executes the Lua script in "filename", with optional arguments (each will be available in the global table arg in Lua).
```
To get started with LuaRT and make your first steps, follow the [Getting started tutorial](https://www.luart.org/doc/install.html)

## :small_blue_diamond:Supporting

There have been many hours of hard work put into LuaRT. Your support will be greatly appreciated!

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/O5O2LKD6Q)

## :small_blue_diamond:Links
  
- :house_with_garden: [LuaRT Homepage](https://www.luart.org/index.html)
- :speech_balloon: [LuaRT Community](https://community.luart.org)
- :book: [LuaRT Documentation](https://www.luart.org/doc/index.html)
  
## :small_blue_diamond:License
  
LuaRT is copyright (c) 2025 Samir Tine.
LuaRT is open source, released under the MIT License.
See full copyright notice in the LICENSE.txt file.

[title]: examples/ui/LuaRT.png
[banner]: https://luart.org/img/features.png
