# Retro-Consoles

Packages every retro console core WinNative uses into a single archive.

The workflow pulls each core from its own fork's `latest` release, adds the
Dolphin and ARMSX2 runtime data at their pinned commits, and publishes
`retro-consoles.tzst` plus a SHA-256 alongside it.

WinNative downloads that archive on demand from Settings > Retro, so the app
repository carries no core binaries or emulator data.

Layout inside the archive:

    cores/    every arm64-v8a core and emucore .so
    data/     dolphin-emu/Sys and armsx2 runtime assets
