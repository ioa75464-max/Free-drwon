# Free drawing — Phase 7 Release Checklist

## Implemented in Phase 7

- Large image imports are downsampled to a maximum practical dimension of 4096 px before being retained in workspace memory.
- Application memory-pressure callbacks are registered so the app can react to Android low-memory signals.
- FDR/autosave continues to use temporary-file replacement rather than writing directly over the last autosave.
- Export remains independent from the Compose viewport and excludes reference windows and UI.
- Workspace UI remains full-screen and floating rather than reserving permanent sidebars.
- README and project status were updated to distinguish implemented functionality from unverified device-only checks.

## Required validation on a real Android environment

The current execution environment does not contain Android SDK, Android Studio, adb, or a complete Gradle wrapper installation. Therefore these checks are intentionally marked **NOT RUN HERE**, not falsely marked as passed.

1. Gradle debug build.
2. Gradle release build.
3. Install APK on a physical Android phone.
4. Draw multiple strokes and switch brushes.
5. Change RGB, opacity and brush size.
6. Undo and redo.
7. Create, hide, lock, reorder, duplicate and merge layers.
8. Import a large image and verify memory behavior.
9. Add multiple references; minimize, hide, restore and remove them.
10. Zoom, pan and rotate the canvas.
11. Save `.fdr`, close the app, reopen and load the project.
12. Kill the app and verify autosave recovery.
13. Export transparent PNG.
14. Export JPEG and WEBP.
15. Hide the toolbar completely and verify drawing remains available.
16. Rotate the device and verify document integrity.
17. Test with multiple large layers/references for memory pressure.
18. Capture frame-time and memory metrics with Android Studio Profiler.

## Release decision

**Source/package status:** prepared for final Android build validation.

**APK status:** not generated in this environment because the Android build toolchain is unavailable.

A release APK should only be called final after the real Gradle build and physical-device checks above pass.
