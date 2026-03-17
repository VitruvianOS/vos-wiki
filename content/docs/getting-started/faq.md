---
title: "FAQ"
weight: 4
---

# FAQ

Common questions and troubleshooting for developers running VitruvianOS.

## How do I start the desktop environment?

From the `generated.x86` folder, run the servers in order:

```bash
./src/apps/testharness/clean_shm.sh
./generated.x86/src/servers/registrar/registrar > registrar.out &
sleep 1
./generated.x86/src/servers/app/app_server > app_server.out &
sleep 2
../generated.x86/src/servers/input/input_server > input_server.out &
sleep 1
./generated.x86/src/apps/deskbar/Deskbar > deskbar.out &
```

## app_server fails with "could not initialize font manager"

The app_server cannot find the fonts. Make sure the fonts folder is present at `/os/system/data`.

## I launched the desktop but it looks like it quit immediately

This is expected behavior. The desktop may appear to close but it is running correctly in the background. Check the `*.out` log files for status.
