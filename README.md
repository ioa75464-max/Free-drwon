# Free drawing

## Current status — Phase 7: Optimization and Release Preparation

Phase 7 hardening is applied to the Phase 6 source. Large imported images are downsampled before being retained in memory, the Application observes Android memory-pressure callbacks, autosave continues to use safe temporary replacement, and release validation documentation is included.

The source is release-prepared, but this environment cannot honestly claim a final APK or physical-device validation because Android SDK, Android Studio, adb, and a complete Gradle wrapper are unavailable. See `RELEASE_CHECKLIST.md` for the exact final validation sequence.

## Current status — Phase 5: Advanced Tools

Implemented in the workspace source:
- Expanded brush/tool set: Basic Brush, Fine Liner, Soft Brush, Hard Brush, Airbrush approximation, Marker, Watercolor-style approximation, Pencil, Ink and Eraser.
- Reusable per-tool brush settings for size, opacity, hardness, flow, smoothing, spacing and pressure response. The settings are kept independently per selected tool where changed.
- Shape tools: Line, Rectangle and Ellipse. Shapes are stored as document elements instead of being flattened into the screen.
- Selection tool with document-space marquee selection and selection highlighting for strokes, shapes and imported artwork.
- Eyedropper for sampling colors from vector shape elements. Stroke/image pixel sampling remains limited because the current phase does not yet have a rasterized document sampling pipeline.
- Canvas transformations: 90-degree rotation, horizontal flip, vertical flip and reset. Transformations are applied through the document viewport transform rather than changing the stored stroke coordinates.
- Improved eraser hit testing with configurable brush-size radius and support for removing nearby strokes and shapes.
- Custom color palette: add the current color and quickly select stored colors.
- Phase 4 image/reference behavior was preserved and hardened: artwork images are rendered for every visible layer, reference windows support visible/minimized/hidden states, and image/reference rotation is visually applied.
- Layer merge now preserves strokes, shapes and imported artwork elements instead of dropping image content.

## Phase 4 — Images and References

Implemented:
- Android image import through the system picker for PNG/JPG/JPEG/WEBP-compatible content providers.
- Multiple independent reference images.
- Reference move, pinch-scale, rotation, opacity, hide/restore, minimize and removal.
- Separate artwork image objects inside drawing layers.
- Artwork image move, scale, rotation, opacity and deletion.
- References remain outside ordinary artwork layers and are not rendered by the artwork renderer.

## Phase 3 — Layer System

Implemented:
- Independent layers with their own strokes, shapes and imported images.
- Create, delete, rename, duplicate, hide/show, lock/unlock, reorder, opacity, clear and merge-down.
- Layer-aware stroke undo/redo.

## Phase 2 — Drawing Engine

Implemented:
- Jetpack Ink 1.0.0 integration.
- Real-time freehand drawing.
- Brush size, opacity and RGB color controls.
- Undo/redo for completed stroke changes.
- Basic Pencil/Ink behavior, now expanded by Phase 5's brush system.

## Important limitations and verification status

- This environment does not contain Android SDK, Android Studio, adb, or a complete Gradle wrapper installation, so an Android APK build and physical-device test cannot honestly be claimed here.
- Phase 5's advanced brushes are intentionally documented as approximations where the current Ink stock-brush API does not expose a full texture engine. They are not being presented as identical to a professional raster brush engine.
- The eyedropper currently samples stored vector shape colors. Full pixel sampling from arbitrary strokes and imported images should be completed when the raster/cache pipeline is introduced.
- The custom palette is currently workspace-session state. Durable palette persistence belongs with the storage/preferences work in the later saving phase.
- Native `.fdr` project serialization, autosave/recovery and final PNG/JPEG/WEBP export remain Phase 6.
- Performance profiling, memory stress testing, APK generation and release validation remain Phase 7.

## Architecture note

The workspace keeps strokes, shapes, imported artwork and references as separate document concepts. References are not part of ordinary drawing layers. The canvas uses a viewport transform for zoom, pan, rotation and flips so the stored document coordinates are not rewritten merely because the user changes the view.

## Phase 6 — Saving & Export

Implemented in this build:

- Native `.fdr` project container using a versioned ZIP archive.
- Project manifest with layer order/state, canvas viewport transforms, palettes, shapes, image metadata, references, and Ink stroke data.
- Ink `StrokeInputBatch` serialization for stored strokes, with brush metadata required to reconstruct the current stock-brush families.
- Imported artwork and reference bitmaps are packaged as PNG assets inside the FDR archive.
- Save / Save As through Android document creation.
- Open project through Android document selection.
- Automatic internal autosave with temporary-file replacement.
- Autosave recovery on application startup.
- Unsaved-state indicator.
- Export to PNG, JPEG, and WEBP.
- Export resolution controls.
- Export rendering excludes references and interface elements.

### Phase 6 limitations

- The current project source still uses a compact single workspace presentation file; deeper architectural separation can be performed during Phase 7.
- Stroke serialization stores the brush family as the current stock family classification (marker or pressure pen) rather than arbitrary future custom brush-family assets. Custom brush preset persistence will need a versioned brush registry when fully custom brush families are introduced.
- Android SDK / Android Studio / adb are not available in this environment, so no real APK build or physical-device test is claimed.
