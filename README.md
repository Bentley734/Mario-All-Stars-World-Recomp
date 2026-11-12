# Mario All-Stars + World Recomp

<a href="https://recomplauncher.com"><picture><source media="(prefers-color-scheme: dark)" srcset="https://recomplauncher.com/assets/press/launcher/badge-light.svg"><img alt="Get it on RecompLauncher" src="https://recomplauncher.com/assets/press/launcher/badge-dark.svg" height="48"></picture></a>

A port of the USA five-game combination cartridge. Open the Windows or macOS
app and choose your own headerless USA `.sfc` or `.smc`. In RecompLauncher,
select the same cartridge in Easy Setup and press Play. The release provides
window mode, audio and volume controls in the launcher.

The SNESRecomp analyzer produces code from your own dump. The runner executes
translated routines alongside cycle-sensitive SNES code and hardware.

## Generate and build

Download the source ZIP from the [latest GitHub release](https://github.com/Bentley734/Mario-All-Stars-World-Recomp/releases/latest) before following these steps.

1. Obtain the [SNESRecomp v0.4.4-alpha source](https://github.com/mstan/snesrecomp/tree/v0.4.4-alpha).
2. With Python and Cargo available, run `bash tools/generate.sh /path/to/snesrecomp /path/to/combo.sfc`.
3. Configure with `cmake -S . -B build -DSNESRECOMP_ROOT=/path/to/snesrecomp -DCMAKE_BUILD_TYPE=Release`.
4. Build with `cmake --build build --target MarioAllStarsWorldRecomp --parallel`.

For desktop and iOS, point `SNESRECOMP_SDL3_SOURCE_DIR` at SDL 3.4.10 to build it from
source. Windows was cross-compiled for x86-64 with MinGW. Apple targets were built for
arm64, with deployment targets macOS 13 and iOS 16. For iOS, configure with
`-DCMAKE_SYSTEM_NAME=iOS -DCMAKE_OSX_SYSROOT=iphoneos`. The app exports an unsigned IPA.

Package the desktop builds with `bash tools/package-desktop.sh /path/to/windows-build /path/to/macos-build /path/to/snesrecomp /path/to/sdl /path/to/output`.
Each versioned ZIP includes `recomplauncher.json` at its root. Windows stores
settings and saves alongside the program. macOS uses `~/Library/Application Support/Mario All-Stars + World`.
On macOS, an empty `portable.txt` beside the app keeps those files with the app
when the install folder is writable.

PS5 needs a Prospero toolchain and a release build of the PS5 SDL2 port. Configure with
its CMake toolchain file and `-DPS5_SDL2_BUILD_DIR=/path/to/sdl2-build`. It produces
a test ELF. Console verification and an installable store package remain separate work.

The matching headerless ROM has 2,621,440 bytes, CRC32 `F84305B1` and SHA-256
`a8806bfe07cd3c9945d9fd3fcea932ae1cd671cab5cae12bb7a2ae726cbf9175`.
Use the USA combination cartridge with Super Mario World in the menu.

## Play

Open the app and choose your cartridge dump on desktop. On iPhone or iPad,
copy the file into the app's folder in Files, then reopen the app. On a PS5
test setup, place the ROM alongside the executable with the exact name
`Super Mario All-Stars + Super Mario World (USA).sfc`.

Open the pause menu with Escape, Start + Select on a mapped controller, or
Pause on the iOS touch overlay. Controls supports keyboard, mouse buttons,
controller buttons and controller triggers for both seats. Cheat availability
depends on the selected game and scene. Supported effects include infinite
lives, a frozen timer, star power and heart refill in Super Mario Bros. 2.

## Gameplay validation

Version 0.1.1 fixes the cartridge handoff that stalled Super Mario Bros. 3
and Super Mario World. The handoff and those two games use the interpreter
to preserve their interrupt and audio timing. All five games reached their
first playable scenes on macOS with the exact USA cartridge. Both fixed
games reached gameplay and rendered at 32:9 in the Windows build through
CrossOver. The iPhone simulator rendered SMB3 at 32:9 and the iPad simulator
rendered Super Mario World at 32:9. The iOS IPA needs signing for a physical
device. The PS5 archive contains a cross-built ELF for hardware testing.

## Widescreen status

Choose **Video → 32:9 Terrain Preview** in the pause menu. Verified SMB3
plains stages and ordinary horizontal Super Mario World stages use a 682×224
logical field with the original pixel proportions. SMB3 World 1-1 shows
Goombas ahead of the camera. Yoshi's Island 1 shows Banzai Bills, Rexes and
flying blocks. Normal activation timing still controls movement and
collisions. Unsupported scenes use the original view. The source ZIP includes widescreen notes in `docs/WIDESCREEN.md`.

The source archive excludes the cartridge and all generated game code.
