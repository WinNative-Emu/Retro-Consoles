# Credits and provenance

Everything in `retro-consoles.tzst` is someone else's work. This records what
each piece is, where it came from, and under what terms it ships, so that the
bundle can be audited without reading the build script.

Each entry is built from a fork under `WinNative-Emu`, which syncs from its
upstream daily. The fork exists so builds are reproducible and so upstream is
never asked to carry WinNative-specific patches; it is not a claim of
authorship.

## Libretro cores

Listed in `cores.list`. Each is a fork of the upstream libretro core and ships
as a single `.so`:

| Core | Upstream | Licence |
| --- | --- | --- |
| FCEUmm | libretro/libretro-fceumm | GPL-2.0 |
| Snes9x | libretro/snes9x | Snes9x non-commercial |
| Gambatte | libretro/gambatte-libretro | GPL-2.0 |
| mGBA | libretro/mgba | MPL-2.0 |
| Genesis Plus GX | libretro/Genesis-Plus-GX | Genesis Plus GX licence |
| Beetle PSX | libretro/beetle-psx-libretro | GPL-2.0 |
| gpSP | libretro/gpsp | GPL-2.0 |
| Mupen64Plus-Next | libretro/mupen64plus-libretro-nx | GPL-2.0 |

## Emulators

| Component | Upstream | Licence | Notes |
| --- | --- | --- | --- |
| ARMSX2 | ARM-friendly PCSX2 fork | GPL-3.0 | `libemucore_*.so` plus its runtime assets |
| Dolphin | dolphin-emu/dolphin | GPL-2.0-or-later | `libmain.so` plus `Data/Sys` |

## The Gen 1 3D path

Listed in `data.list`. This is not an emulator: gen1recomp is a
reimplementation of the games in Lua, and it runs the player's own verified ROM
data rather than executing the ROM.

### gen1recomp

- **Upstream:** bryanthaboi/gen1recomp
- **Copyright:** BOIS CLUB GAMES, LLC
- **Licence:** MIT
- **Ships as:** `data/gen1recomp/game.love` plus the arm64 LÖVE runtime
  (`lib/libc++_shared.so`, `liblove.so`, `libmpg123.so`, `libopenal.so`)

The fork carries three build-time patch scripts, applied during the build
rather than committed, so the daily upstream sync never conflicts with them:

- `winnative_sdl_namespace.sh` — moves SDL's Java glue out of `org.libsdl.app`,
  which ARMSX2 also ships from a different SDL version.
- `winnative_boot_args.sh` — lets the ROM path and game version arrive on the
  command line, and defaults the engine's own touch overlay off.
- `winnative_bridge.sh` — wires in `src/core/WinNativeBridge.lua`, which lets
  WinNative's menu read and change the engine's settings.

`WinNativeBridge.lua` is new work in the fork and is offered upstream under the
same MIT terms as the project it lives in.

The LÖVE runtime it links against is © 2006-2024 LÖVE Development Team, zlib
licence, and bundles SDL2 (zlib), OpenAL Soft (LGPL-2.1) and mpg123 (LGPL-2.1).

### Dramatic Shape Voxel Mod

- **Upstream:** DramaticShape/DramaticShapeVoxelMod
- **Ships as:** `data/gen1recomp-mods/DRAMATIC_SHAPE.zip`
- **Terms:** the repository carries no licence file. Permission to distribute
  it in this bundle was given directly by the project's owner. That permission
  is the only basis on which it ships here — the absence of a licence file
  means no general grant exists, so anyone redistributing this bundle or
  reusing the mod outside it needs their own permission from the owner.

If that permission is ever withdrawn, removing the `DramaticShapeVoxelMod` line
from `data.list` is sufficient: the engine runs without the mod, in 2D.

## ROMs

None. No game ROM is included in this bundle or in any component of it. The
Gen 1 3D engine imports a ROM the player supplies and verifies it by SHA-1
before use.
