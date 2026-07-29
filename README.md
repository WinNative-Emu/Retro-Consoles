# Retro-Consoles

Packages every retro console core WinNative uses into a single archive.

The workflow pulls each core from its own fork's `latest` release, adds the
Dolphin and ARMSX2 runtime data at their pinned commits, and publishes
`retro-consoles.tzst` with a SHA-256 and a `bundle-info.json` descriptor.

WinNative downloads that archive on demand from Settings > Retro, so the app
repository carries no core binaries or emulator data.

Layout inside the archive:

    cores/    every arm64-v8a core and emucore .so
    data/     dolphin-emu/Sys and armsx2 runtime assets

## Schedule

Every core fork syncs its upstream and rebuilds daily at 17:00
America/New_York; this repository packages them an hour later, at 18:00. Both
crons are fixed UTC (21:00 and 22:00), so the local times shift by an hour
outside daylight saving.

A scheduled run does no work unless there is work to do. Each fork only rebuilds
when the upstream merge actually brought commits, and this repository fingerprints
every core's published asset first (`sources.sha256`) and exits before downloading
anything when nothing has moved.

## Update detection

`bundle-info.json` carries the tag, build date, SHA-256 and size. WinNative keeps
a copy of the installed one and compares the SHA-256 against the published one.

The archive is packed reproducibly (`tar --sort=name --mtime=@0 --owner=0
--group=0 --numeric-owner`) and no run-specific file goes inside it, so identical
cores produce an identical archive. A run whose SHA-256 matches what is already
published skips the release entirely, which is what stops the weekly rebuild from
showing users an update that contains nothing new.

## Adding or removing a core

Edit `cores.list` (`fork repo|release asset`). The four adrenotools hook
libraries Dolphin needs are deliberately absent: they are `dlopen`ed by name out
of the APK's own library directory, never from the bundle.
