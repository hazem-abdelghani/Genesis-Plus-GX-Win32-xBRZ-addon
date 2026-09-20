xBRZ add-on for the Genesis Plus GX Windows GUI
===============================================

This folder is an OPTIONAL add-on. It is not part of the main source tree and
the program builds and runs without it. With it, Video > Render Filter gains
"xBRZ 2x" to "xBRZ 6x".

INSTALLING
----------
Either unpack the add-on zip INSIDE the win32/ folder of the source tree, or,
from the root of the source tree, clone the add-on repository into it:

    git clone https://github.com/hazem-abdelghani/Genesis-Plus-GX-Win32-xBRZ-addon win32/xbrz

Either way this file must end up at

    win32/xbrz/README-xbrz.txt        (and win32/xbrz/xbrz.cpp exists)

then build with XBRZ=1, from win32/:

    make -f Makefile.win32 CROSS=x86_64-w64-mingw32- XBRZ=1        (64-bit)
    make -f Makefile.win32 CROSS=i686-w64-mingw32-   XBRZ=1        (32-bit)

You also need a C++ compiler (g++, from the same MinGW-w64 package). To go
back, build without XBRZ=1 (the default), or delete the win32/xbrz/ folder.
Asking for XBRZ=1 without this folder stops with a message saying so.

The main repository's .gitignore lists win32/xbrz/, so if you keep this folder
in a working copy it will not be committed by accident.

LICENCE -- READ THIS BEFORE YOU SHARE A BINARY
----------------------------------------------
Everything in this folder is under the GNU General Public License, version 3
(COPYING): the xBRZ files (see the headers in them, which also carry the
author's exception allowing linking with MAME) and the two glue files
xbrz_glue.cpp / xbrz_glue.h, which exist only to drive xBRZ and are offered
under the same terms.

The rest of the project is under Genesis Plus GX's licence, which does not
allow commercial use. Those two licences are not compatible: the GPL does not
allow a combined work to carry extra restrictions like that. In practice:

  * Building and using the program yourself, with this add-on, is fine.
  * Distributing an EXECUTABLE that contains xBRZ to other people is where the
    conflict arises. Build without XBRZ=1 for anything you share. The
    executables published with the main project are built that way.
  * This add-on can be shared on its own, as source, under the GPL, provided
    COPYING and these notices stay with it.
  * If you distribute a build that includes xBRZ anyway, the GPL requires you
    to make the corresponding source available and to keep these notices.

This is a description of the situation, not legal advice.

WHAT IT IS
----------
xBRZ ("scale by rules") is a pixel-art upscaling filter written by Zenju. The
files here are the xBRZ scaler as bundled in the PPSSPP project
(https://github.com/hrydgard/ppsspp, directory ext/xbrz; fetched 2026-09-19),
which is taken from Zenju's HqMAME sources:

    xbrz.cpp, xbrz.h     the scaler
    xbrz_config.h        its settings struct (byte-identical to the original
                         file, which PPSSPP names config.h; renamed here so it
                         cannot be confused with the frontend's own config.h)
    xbrz_glue.cpp, .h    RGB565 <-> xBRZ pixel conversion (added for this
                         project; the only code here that is not Zenju's)
    COPYING              the GNU General Public License, version 3

The algorithm is unchanged. It is the real xBRZ, at its default settings, and
it produces the same output as the untouched original: this was checked bit for
bit at every scale factor from 2x to 6x, on pixel art, random noise and edge
cases.

WHAT WAS CHANGED
----------------
Only memory handling and includes; each change is marked "MODIFIED" in the
files themselves:

  1. Removed the PPSSPP build-system include (ppsspp_config.h) and its iOS
     special case; config.h -> xbrz_config.h.
  2. xBRZ keeps a 256*256*256-entry colour-distance table (64 MB of floats)
     that it built on first use inside a function-local static std::vector and
     kept for the life of the process. It is now a malloc'd buffer with
     prepareTables() / releaseTables(), so the frontend can build it when an
     xBRZ filter is selected and give the 64 MB back when another filter is
     chosen. As a side effect the code no longer needs any C++ runtime
     library, so the executable still imports only DLLs that ship with Windows.
     The table's contents and the way it is indexed are exactly as before.

PIXEL LAYOUT
------------
This copy reads red from the LOW byte of each 32-bit pixel (0x00BBGGRR), not
the usual 0x00RRGGBB. Its colour-distance table weights red, green and blue
differently, so xbrz_glue.cpp packs pixels the way this code reads them.
