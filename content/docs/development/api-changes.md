---
title: "API Changes"
weight: 2
---

# API Changes

Differences between the VitruvianOS native API and BeOS/Haiku.

## Images

- `image_id` values are local to the application.

## Processes

- `start_watching_system` must be called with superuser privileges.

## Threads

- Thread suspension is forbidden. Any application calling `suspend_thread` will be killed.

## Filesystem

- See [Filesystem Layout]({{< relref "filesystem-layout" >}}) for directory structure changes.

## libbe Annotations

This section tracks generic differences in the C++ API provided by `libbe`. It will be expanded as the compatibility layer matures.
