﻿# | LuaRT - A Windows programming framework for Lua
# | Copyright (c) Tine Samir 2024
# | See Copyright Notice in LICENSE.TXT
# |--------------------------------------------------------------
# | LuaRT Core Makefile for the toolchain, the Core framework and the tools
# | You can prefix all the following tasks with PLATFORM=x64|x86  
# | Don't forget to get all submodules :
# | git clone --recurse-submodules git://github.com/samyeyo/LuaRT.git
# |--------------------------------------------------------------
# | Usage (default release build)			 : make
# | Usage (debug build) 		  			 : make debug
# | Usage (release with tools)		 		 : make all
# | Usage (build setup executable)	  		 : make setup
# | Usage (build tools)				  		 : make tools
# | Usage (clean all)	 				 	 : make clean
# |-------------------------------------------------------------
# | Or you can use default release build for any platform 
# | Usage (default x64 release build)		 : make x64
# | Usage (default x86 release build)		 : make x86
# |-------------------------------------------------------------

#---- Platform selection, leave blank to autodetect
#---- x86 for 32bits lua54.dll, liblua54.a and executables
#---- x64 for 64bits lua54.dll, liblua54.a and executables
VERBOSE= 2>&1 >nul

PLATFORM=

!if ([cl /? 2>&1 | findstr /C:"x86" > nul] == 0)
PLATFORM=x86
!else
PLATFORM=x64
!endif


#---- Configuration
RM= del /Q
CC= cl
LD= link
LB= lib

DEST= ..\..\bin


!if "$(PLATFORM)" == "x64"
_ARCH = x64
!else
_ARCH = x86
!endif

#---- LuaRT version
LUART_MAJOR= 2
LUART_MINOR= 0
LUART_RELEASE= 0
!if "$(VERSION)" == ""
VERSION=$(LUART_MAJOR).$(LUART_MINOR).$(LUART_RELEASE)
!endif
DEFINES_RES= /d LUART_MINOR="$(LUART_MINOR)" /d LUART_MAJOR="$(LUART_MAJOR)" /d LUART_RELEASE="$(LUART_RELEASE)"

!if "$(BUILD)" == "debug"
CFLAGS = /nologo /D_WIN32_WINNT=0x0603 /DLUA_BUILD_AS_DLL /DLUA_COMPAT_5_3 /DLUA_ARCH=\"$(_ARCH)\" /DZIP_SHARED /DZIP_BUILD_SHARED /DLUART_MINOR=$(LUART_MINOR) /DLUART_MAJOR=$(LUART_MAJOR) /DLUART_RELEASE=$(LUART_RELEASE) /I"..\..\include" /I"." /Z7 /Od
LDFLAGS= /nologo /DEBUG /NODEFAULTLIB:msvcrt
VERBOSE= 
PROGRESS=
!else
CFLAGS = /nologo /D_WIN32_WINNT=0x0603 /DLUA_BUILD_AS_DLL /DLUA_COMPAT_5_3 /DLUA_ARCH=\"$(_ARCH)\" /DZIP_SHARED /DZIP_BUILD_SHARED /DLUART_MINOR=$(LUART_MINOR) /DLUART_MAJOR=$(LUART_MAJOR) /DLUART_RELEASE=$(LUART_RELEASE) /I"..\..\include" /I"." /MD /Gy /GF /Oi /O1 /GA
LDFLAGS= /nologo /OPT:REF /OPT:ICF
PROGRESS= "■"
!endif

DLL_LIBS =	shlwapi.lib advapi32.lib gdi32.lib comctl32.lib kernel32.lib user32.lib oleaut32.lib ole32.lib credui.lib crypt32.lib wininet.lib shell32.lib

LIBS	= 		..\..\lib\lua54.lib ole32.lib comctl32.lib shell32.lib
LIBS_STATIC	= 	..\..\lib\lua54-static.lib ole32.lib comctl32.lib shell32.lib shlwapi.lib advapi32.lib gdi32.lib comctl32.lib kernel32.lib user32.lib oleaut32.lib crypt32.lib
UI_LIBS = 		gdi32.lib uxtheme.lib shell32.lib user32.lib kernel32.lib dwmapi.lib windowscodecs.lib comdlg32.lib ole32.lib shlwapi.lib oleaut32.lib shcore.lib 


#---- Source files
LUA_A=		..\..\bin\lua54.dll
LUA_O=		lua\lapi.obj lua\lcode.obj lua\lctype.obj lua\ldebug.obj lua\ldo.obj lua\ldump.obj lua\lfunc.obj lua\lgc.obj lua\llex.obj lua\lmem.obj lua\lobject.obj lua\lopcodes.obj lua\lparser.obj lua\lstate.obj lua\lstring.obj lua\ltable.obj lua\ltm.obj lua\lundump.obj lua\lvm.obj lua\lzio.obj	
LIB_O=		lua\lauxlib.obj lua\lbaselib.obj lua\lcorolib.obj lua\ldblib.obj lua\lmathlib.obj lua\loadlib.obj lua\ltablib.obj string\string.obj string\lstrlib.obj sys\sys.obj console\console.obj lua\liolib.obj lua\loslib.obj lua\lutf8lib.obj compression\compression.obj compression\Zip.obj compression\lib\zip.obj lrtapi.obj lrtobject.obj sys\Date.obj sys\File.obj sys\Pipe.obj sys\Directory.obj sys\Buffer.obj sys\Com.obj lembed.obj sys\async.obj sys\Task.obj
UI_O=  		ui\ui.obj ui\Widget.obj ui\Entry.obj ui\Items.obj ui\Menu.obj ui\Window.obj ui\Darkmode.obj ui\DragDrop.obj

BASE_O= 	$(LUA_O) $(LIB_O)
ALL_O= 		$(BASE_O)

LUART_T=	..\..\bin\luart.exe
LUAWRT_T=	..\..\bin\wluart.exe
RTC_T=		..\..\bin\rtc.exe
ALL_T= 		$(LUA_A) $(LUART_T) $(LUAWRT_T) $(RTC_T)

!if "$(BUILD)" != "debug"
!message 
!message --------------------  Building LuaRT $(LUART_MAJOR).$(LUART_MINOR).$(LUART_RELEASE) for $(PLATFORM)  --------------------
!endif

all: $(RTC_T) toolchain

debug:
	@$(MAKE) /nologo "BUILD=debug"

utf8:
	@chcp 65001 >nul 2>&1

default: toolchain

infolua: utf8
	@cmd /c echo.
	@echo|set /p dummy="▸  Building Lua 5.4.7 VM          "

infocore:
	@cmd /c echo.
	@echo|set /p dummy="▸  Building LuaRT core framework  "

infostatic:
	@cmd /c echo.
	@echo|set /p dummy="▸  Building static interpreters   "

infotoolchain:
	@cmd /c echo.
	@echo|set /p dummy="▸  Building LuaRT toolchain       "

infosetup: utf8
	@cmd /c echo.
	@echo|set /p dummy="▸  Building LuaRT installer       "

.cpp.obj:
	@$(CC) /DLUA_BUILD_AS_DLL $(CFLAGS) /c /Fo$@ $< $(VERBOSE) /EHsc
	@echo|set /p dummy=$(PROGRESS)

.c.obj: 
	@$(CC) /DLUA_BUILD_AS_DLL $(CFLAGS) /c /Fo$@ $< $(VERBOSE)
	@echo|set /p dummy=$(PROGRESS)

lua: infolua $(LUA_O) 

core: infocore $(LIB_O) $(UI_O)

toolchain: lua core $(LUA_A) infotoolchain $(LUART_T) $(LUAWRT_T) static

lua\lvm.obj: 
!if "$(BUILD)" == "debug"
	@$(CC) /nologo /c /Z7 /Fo$@ lua\lvm.c $(VERBOSE)
!else
	@$(CC) /nologo /MD /Gy /GF /Oi /GA /c /O2 /Fo$@ lua\lvm.c $(VERBOSE)
!endif

..\..\lib\lua54-static.lib: $(LUART_T) 
	@..\..\bin\luart.exe lua\static\export_h.lua $(VERBOSE)
	@echo|set /p dummy=$(PROGRESS)
	@$(CC) /c lua\static\lib.cpp $(CFLAGS) /Folua\static\lib.obj $(VERBOSE)
	@echo|set /p dummy=$(PROGRESS)
	@$(LB) lua\static\lib.obj /OUT:lua54-static.lib $(VERBOSE)
	@-copy /Y lua54-static.lib "..\..\lib\lua54-static.lib" >nul
	@echo|set /p dummy=$(PROGRESS)

..\..\bin\luart-static.exe: $(BASE_O)
	@$(CC) /DRTSTATIC /c $(CFLAGS) lua\loadlib.c /Folua\loadlib.obj $(VERBOSE)
	@echo|set /p dummy=$(PROGRESS)
	@rc /fo .\\resource.res $(DEFINES_RES) /r /v /i "lua/" resources/resource.rc >nul 2>&1
	@echo|set /p dummy=$(PROGRESS)
	@$(CC) /DRTSTATIC $(CFLAGS) luart.c $(BASE_O) $(LIBS_STATIC) /link /ignore:4217 $(LDFLAGS) resource.res /OUT:luart-static.exe $(VERBOSE)
	@echo|set /p dummy=$(PROGRESS)
	@-copy /Y luart-static.exe "$(DEST)\luart-static.exe" >nul

..\..\bin\wluart-static.exe: $(BASE_O) $(UI_O)
	@rc /fo .\\resource.res $(DEFINES_RES) /d RTWIN /r /v /i "lua/" resources/resource.rc >nul 2>&1
	@$(CC) /DRTSTATIC /DRTWIN $(CFLAGS) luart.c resource.res $(BASE_O) $(UI_O) $(LIBS_STATIC) $(UI_LIBS) /link /ignore:4217 $(LDFLAGS) /OUT:wluart-static.exe /SUBSYSTEM:windows $(VERBOSE)
	@echo|set /p dummy=$(PROGRESS)
	@-copy /Y wluart-static.exe "$(DEST)\wluart-static.exe" >nul

static: infostatic ..\..\lib\lua54-static.lib ..\..\bin\luart-static.exe ..\..\bin\wluart-static.exe
	@$(CC) /c $(CFLAGS) lua\loadlib.c /Folua\loadlib.obj $(VERBOSE)

tools: infotoolchain toolchain $(RTC_T)
	@echo|set /p dummy=" "

..\..\bin\lua54.dll: $(BASE_O)
	@$(LD) /DLL $(LDFLAGS) $(BASE_O) $(DLL_LIBS) /OUT:$@ $(VERBOSE)
	@echo|set /p dummy=$(PROGRESS)
	@-move /Y ..\..\bin\lua54.lib "..\..\lib\lua54.lib" >nul
	@-$(RM) ..\..\bin\lua54.exp >nul 2>&1

$(LUART_T): $(LUA_A)
	@rc /fo .\\resource.res $(DEFINES_RES) /r /v /i "lua/" resources/resource.rc >nul 2>&1
	@echo|set /p dummy=$(PROGRESS)
	@$(CC) $(CFLAGS) luart.c /link $(LDFLAGS) resource.res $(LIBS) /OUT:$@ $(VERBOSE)
	@echo|set /p dummy=$(PROGRESS)
	@-$(RM) ..\..\bin\luart.exp >nul 2>&1
	@-$(RM) ..\..\bin\luart.pdb >nul 2>&1
	@-$(RM) ..\..\bin\luart.lib >nul 2>&1

$(LUAWRT_T): $(LUA_A) $(UI_O)
	@rc /fo .\\resource.res $(DEFINES_RES) /r /v /d RTWIN /i "lua/" resources/resource.rc >nul 2>&1
	@echo|set /p dummy=$(PROGRESS)
	@$(CC) /c /DRTWIN $(CFLAGS) luart.c /Fowluart.obj $(VERBOSE)
	@$(LD) $(LDFLAGS) resource.res wluart.obj $(UI_O) $(LIBS) $(UI_LIBS) /OUT:$@ /SUBSYSTEM:windows $(VERBOSE)
	@echo|set /p dummy=$(PROGRESS)
	@-$(RM) ..\..\bin\wluart.exp >nul 2>&1
	@-$(RM) ..\..\bin\wluart.pdb >nul 2>&1
	@-$(RM) ..\..\bin\wluart.lib >nul 2>&1

$(RTC_T): toolchain static ..\..\tools\rtc\src\rtc.lua
	@-$(RM) $(DEST)\rtc.exe >nul 2>&1
	@-$(RM) $(DEST)\wrtc.exe >nul 2>&1
	@-$(RM) $(DEST)\rtcheck.exe >nul 2>&1
	@-copy $(DEST)\*.exe ..\..\tools\rtc\src >nul
	@cmd /c echo.
	@echo|set /p dummy="▸  Building rtc compiler          "
	@rc /fo .\\resource.res $(DEFINES_RES) /r /v /d RTC /i "lua/" resources/resource.rc >nul 2>&1
	@echo|set /p dummy=$(PROGRESS)
	@$(CC) $(CFLAGS) /D RTC luart.c /link /ignore:4217 $(LDFLAGS) resource.res $(BASE_O) $(LIBS_STATIC) /OUT:luart.exe $(VERBOSE)
	@luart.exe ..\..\tools\rtc\src\rtc.lua ..\..\tools\rtc\src\rtc.lua -o rtc.exe ..\..\tools\rtc\src $(VERBOSE)
	@echo|set /p dummy=$(PROGRESS)
	@rc /fo .\\resource.res $(DEFINES_RES) /r /v /d RTC /d RTWIN /i "lua/" resources/resource.rc >nul 2>&1
	@echo|set /p dummy=$(PROGRESS)
	@$(CC) /c /DRTWIN /DRTC $(CFLAGS) luart.c /Fowluart.obj $(VERBOSE)
	@$(LD) /ignore:4217 $(LDFLAGS) resource.res wluart.obj $(BASE_O) $(UI_O) $(UI_LIBS) $(LIBS_STATIC) /OUT:wluart.exe /SUBSYSTEM:windows $(VERBOSE)
	@echo|set /p dummy=$(PROGRESS)
	@luart.exe ..\..\tools\rtc\src\rtc.lua -o wrtc.exe -w -i ..\..\tools\rtc\src\img\rtc.ico ..\..\tools\rtc\src\rtc.lua ..\..\tools\rtc\src $(VERBOSE)
	@-copy /Y rtc.exe "$(DEST)\rtc.exe" >nul	
	@-copy /Y wrtc.exe "$(DEST)\wrtc.exe" >nul	
	@-$(RM) luart.exe >nul 2>&1
	@-$(RM) wluart.exe >nul 2>&1
	@-$(RM) ..\..\tools\rtc\src\*.exe >nul 2>&1 

buildtools: clean $(RTC_T)
	@cd ..\..\tools\RTBuilder
	@nmake.bat "LUART_PATH=%CD%/../.."
	@cd ..\..\src\core
	@echo|set /p dummy="▸  Building QuickRT/LuaRT Studio  "
	@$(CC) /DAIO /DLUA_BUILD_AS_DLL $(CFLAGS) /c /Folrtapi.obj lrtapi.c $(VERBOSE)
	@$(CC) /DUI /DLUA_BUILD_AS_DLL $(CFLAGS) /c /Foui\ui.obj ui\ui.c $(VERBOSE)
	@echo|set /p dummy=$(PROGRESS)
	@$(LD) /DLL $(LDFLAGS) $(DLL_LIBS) $(UI_LIBS) $(BASE_O) $(UI_O) /ignore:4217 /OUT:lua54.dll $(VERBOSE)
	@echo|set /p dummy=$(PROGRESS)
	@..\..\bin\rtc -i ..\..\tools\QuickRT\contrib\quickrt.ico ..\..\tools\QuickRT\src\quickrt.lua -o ..\..\tools\QuickRT\QuickRT.exe ..\..\tools\QuickRT\src $(VERBOSE)
	@-copy /Y lua54.dll ..\..\tools\QuickRT\lua54.dll >nul	
	@echo|set /p dummy=$(PROGRESS)	
	@$(CC) /DRTCOMPAT /DAIO /DLUA_BUILD_AS_DLL $(CFLAGS) /c /Folrtapi.obj lrtapi.c $(VERBOSE)
	@echo|set /p dummy=$(PROGRESS)
	@$(CC) /DRTWIN /DLUA_BUILD_AS_DLL $(CFLAGS) /c /Fosys\File.obj sys\File.c $(VERBOSE)
	@echo|set /p dummy=$(PROGRESS)	
	@$(LD) /DLL $(LDFLAGS) $(DLL_LIBS) $(UI_LIBS) $(BASE_O) $(UI_O) /ignore:4217 /OUT:lua54.dll $(VERBOSE)
	@echo|set /p dummy=$(PROGRESS)
	@$(CC) /DLUA_BUILD_AS_DLL $(CFLAGS) /c /Folrtapi.obj lrtapi.c $(VERBOSE)
	@echo|set /p dummy=$(PROGRESS)
	@$(CC) /DLUA_BUILD_AS_DLL $(CFLAGS) /c /Fosys\File.obj sys\File.c $(VERBOSE)
	@echo|set /p dummy=$(PROGRESS)	
	@rc /fo .\\resource.res $(DEFINES_RES) /r /v /d RTWIN /i "lua/" resources/resource.rc >nul 2>&1
	@echo|set /p dummy=$(PROGRESS)
	@$(CC) /DUI $(CFLAGS) luart.c /link $(LDFLAGS) resource.res lua54.lib ole32.lib comctl32.lib shell32.lib /OUT:luart.exe  $(VERBOSE)
	@echo|set /p dummy=$(PROGRESS)
	@-xcopy "..\..\tools\LuaRT-Studio\$(PLATFORM)\*" "..\..\tools\LuaRT-Studio" /s /e /Y >nul 2>&1
	@-copy /Y luart.exe ..\..\tools\LuaRT-Studio\bin\luart.exe >nul
	@-copy /Y ..\..\bin\wrtc.exe ..\..\tools\LuaRT-Studio\bin\wrtc.exe >nul
	@-copy /Y lua54.dll ..\..\tools\LuaRT-Studio\bin\lua54.dll >nul	
	@echo|set /p dummy=$(PROGRESS)
	@-$(RM) lua54.dll >nul 2>&1
	-@rd ..\..\tools\LuaRT-Studio\examples /s /q 
	@xcopy ..\..\examples ..\..\tools\LuaRT-Studio\examples\ /s /e /y >nul
	@echo|set /p dummy=$(PROGRESS)
	@xcopy ..\..\modules ..\..\tools\LuaRT-Studio\modules\ /s /e /y >nul
	@echo|set /p dummy=$(PROGRESS)
	@..\..\bin\rtc.exe -s -w -o rtcheck.exe -lnet -i ..\..\setup\img\update.ico ..\..\setup\update.lua ..\..\setup\img\ 2>&1 >nul
	@-copy /Y rtcheck.exe ..\..\tools\LuaRT-Studio\bin\rtcheck.exe 2>&1 >nul
	@-move /Y rtcheck.exe ..\..\bin 2>&1 >nul
	@echo|set /p dummy=$(PROGRESS)
	@cmd /c echo.

setup: buildtools infosetup
	@..\..\bin\luart.exe makedist.lua $(VERSION) $(PLATFORM)
	@echo|set /p dummy=$(PROGRESS)
	@-move /Y LuaRT-$(VERSION)-$(PLATFORM).zip ..\..\LuaRT-$(VERSION)-$(PLATFORM).zip >nul 2>&1
	@-move /Y LuaRT-$(VERSION)-$(PLATFORM).exe ..\..\LuaRT-$(VERSION)-$(PLATFORM).exe >nul 2>&1
	@cmd /c echo.

clean: utf8
	@echo|set /p dummy="▸  Cleaning Lua 5.4 VM... "
	@-$(RM) $(BASE_O) >nul 2>&1
	@echo ✓
	@echo|set /p dummy="▸  Cleaning LuaRT Core framework... "
	@-$(RM) *.tmp >nul 2>&1
	@-$(RM) *.obj >nul 2>&1
	@-$(RM) *.pdb >nul 2>&1
	@-$(RM) *.exp >nul 2>&1
	@-$(RM) *.lib >nul 2>&1
	@-$(RM) *.ilk >nul 2>&1
	@-$(RM) ..\..\bin\*.pdb >nul 2>&1
	@-$(RM) ..\..\bin\*.exp >nul 2>&1
	@-$(RM) ..\..\bin\*.lib >nul 2>&1
	@-$(RM) lua54.* >nul 2>&1
	@-$(RM) resource.res >nul 2>&1
	@-$(RM) ..\..\lib\lua54.lib >nul 2>&1
	@-$(RM) $(ALL_T) >nul 2>&1
	@-$(RM) $(LUART_T) >nul 2>&1
	@-$(RM) $(LUAWRT_T)  >nul 2>&1
	@-$(RM) $(UI_O) >nul 2>&1
	@-$(RM) compression\lib\*.obj >nul 2>&1
	@echo ✓
	@cd ..\..\tools\RTBuilder
	@nmake.bat clean >nul 2>&1
	@cd ..\..\src\core
	@echo ✓	@echo|set /p dummy="▸  Cleaning LuaRT toolchain... "
	@-$(RM) $(DEST)\*.exe >nul 2>&1
	@-$(RM) rtc.exe >nul 2>&1
	@-$(RM) wrtc.exe >nul 2>&1
	@-$(RM) luart*.exe >nul 2>&1
	@-$(RM) wluart*.exe >nul 2>&1
	@-$(RM) quickrt.exe >nul 2>&1
	@-$(RM) ..\..\tools\rtc\src\*.exe >nul 2>&1
	@-$(RM) ..\..\setup\luaRT.zip >nul 2>&1
	@-$(RM) ..\..\tools\QuickRT\QuickRT.exe >nul 2>&1
	@-$(RM) ..\..\tools\QuickRT\lua54.dll >nul 2>&1
	@-$(RM) LuaRT-$(VERSION)-x64.* >nul 2>&1
	@-$(RM) LuaRT-$(VERSION)-x86.* >nul 2>&1
	@-$(RM) ..\..\LuaRT-$(VERSION)-x64.* >nul 2>&1
	@-$(RM) ..\..\LuaRT-$(VERSION)-x86.* >nul 2>&1
	@echo ✓
