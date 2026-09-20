# xBRZ add-on for the Genesis Plus GX Windows GUI

An optional add-on for [Genesis Plus GX — Windows GUI](https://github.com/hazem-abdelghani/Genesis-Plus-GX-Win32).
It adds the **xBRZ** pixel-art scaler, as `xBRZ 2x` to `xBRZ 6x` under
**Video → Render Filter**. The main project builds and runs without it.

> **This is GPLv3 code.** The main project is under Genesis Plus GX's
> non-commercial licence, and the two cannot be combined in one distributed
> program. So this lives in its own repository, and **you should not distribute
> an executable that contains it**. Building it in for your own use is fine.
> Details below and in [`README-xbrz.txt`](README-xbrz.txt).

## Install

Get it into the `win32/xbrz` folder of the main project's source tree, either
by cloning from the root of that tree:

```sh
git clone https://github.com/hazem-abdelghani/Genesis-Plus-GX-Win32-xBRZ-addon win32/xbrz
```

or by downloading `Genesis-Plus-GX-xBRZ-addon.zip` from this repository's
**Releases** and unpacking it inside `win32/`. Then build from `win32/` with
`XBRZ=1` (this also needs a C++ compiler, `g++`):

```sh
make -f Makefile.win32 CROSS=x86_64-w64-mingw32- XBRZ=1     # 64-bit
make -f Makefile.win32 CROSS=i686-w64-mingw32-   XBRZ=1     # 32-bit
```

The main project's `.gitignore` already lists `win32/xbrz/`, so a copy here is
never committed to it by accident.

## What is in here

| File | What it is |
|---|---|
| `xbrz.cpp`, `xbrz.h`, `xbrz_config.h` | Zenju's xBRZ scaler, with two small memory-handling changes marked `MODIFIED` in the files |
| `xbrz_glue.cpp`, `xbrz_glue.h` | Converts between the emulator's RGB565 frames and xBRZ's 32-bit pixels |
| `COPYING` | The GNU General Public License, version 3 |
| `README-xbrz.txt` | Installation, what was changed and why, licensing in full |

## Licence

Everything here is under the **GNU GPL, version 3** (`COPYING`). xBRZ is by
Zenju; this copy comes from the version bundled in the PPSSPP project, which
takes it from Zenju's HqMAME sources. The two glue files exist only to drive
xBRZ and are offered under the same terms.

- Using a build you made yourself, with this add-on, is fine.
- Do **not** distribute an executable that contains it: the GPL and the
  non-commercial Genesis Plus GX licence cannot both apply to one program.
  Executables published for the main project are built without it.
- This add-on can be shared on its own, as source, under the GPL, with
  `COPYING` and the notices kept.

This is a description of the situation, not legal advice.
