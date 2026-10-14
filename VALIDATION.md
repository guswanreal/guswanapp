# Guswan Motion 0.2.0 validation

Date: 2026-10-14. Native Android app, package `com.guswan.motion`, versionCode 2, Android 8.0+. Validation device: AOSP Android 11/API 30 x86_64 emulator, 720×1280 at density 280, software emulation without KVM. No physical phone is connected.

## Build and logic

- PASS: `assembleDebug`, `assembleDebugAndroidTest`, `testDebugUnitTest`, `lintDebug` completed. Evidence: `validation/v0.2-build.log` and XML results in `app/build/test-results/testDebugUnitTest`.
- PASS: 35 unit tests, 0 failures/errors/skips (13 project, 8 editor state, 14 advanced animation).
- PASS: lint has 0 errors; four nonblocking KTX convenience suggestions remain.
- PASS: APK v2 signing verified; certificate SHA-256 `a9f73c39a32ba7696bd1328cd25e9dc1a9c837d497c2226708123bac4cd63676` matches the published 0.1 APK. In-place install with `adb install -r` succeeded.
- PASS: production APK excludes the face-video instrumentation fixture. It includes bundled detector models/native code for arm64-v8a, armeabi-v7a, x86 and x86_64.

## Device tests

Seven device tests passed (266.725 seconds). Both UI journeys passed again after palette/corner refinements (167.381 seconds). The final graph sampling uses the same clamped pose interpolation as rendering; its overshoot limits are covered in unit tests. The final APK passed the workspace/rotation regression (105.235 seconds), including live portrait–landscape–portrait rotation while retaining the same Activity and project; no new ANR trace was created. Exact outcomes are recorded in validation-results.json and the artifact logs.

The device suite exercises:

- `AdvancedWorkspaceTest`: Home → new 3D scene → sphere → camera orbit → Graph/Flow Snap → manual Bézier handle → Reverse time remap → saved project → Home → reopen → feature catalogue.
- `EditorFlowTest`: empty project → text → two transform keyframes → duplicate → undo/redo → delete → example → autosave → activity recreation → playback/pause → export choices.
- `AdvancedMediaTest`: independent project files survive store recreation/deletion; actual decoded video frames follow reverse mapping; animated perspective meshes and camera changes alter pixels; the actual bundled ML Kit model detects the moving public-domain portrait fixture and bakes position/XYZ Euler keyframes.
- `VideoExportTest`: native AVC encoder produces one H.264 track, 30 samples, 540×960 output and differing decoded first/late frame pixels.

An earlier run exposed a test selector that tried to find an uncomposed LazyColumn item. The selector now scrolls the list to the item. A System UI ANR in software emulation also interrupted a back-button check. The ANR was dismissed and the device suite rerun; explicit close controls were added to app sheets. Failed/aborted runs are not counted as passing final evidence.

Do not run the Android CLI layout instrumentation concurrently with Compose tests: both need UiAutomation. One inspection attempt during a test run failed to connect and its helper process crashed; that helper is separate from this app.

## Delivery gate

Statuses below apply to the evidence actually gathered, not an assertion of exhaustive coverage.

- R-02 PASS: source scan finds no em dash in shipped app text.
- R-03 PASS for observed targets: screenshots verify 720×1280 portrait, 1280×720 landscape and Home/guide at 200% font. Body content scrolls instead of overflowing. Other device/inset combinations remain unverified.
- R-17 PASS: no invented product statistics. Layer counts, progress and track sample counts derive from app state.
- R-18 PASS: no testimonials or fabricated customer/person content.
- R-23 PASS: user explicitly requested Home/navigation, studio objects and tracking. Product thumbnails render real saved projects. The attributed portrait clip is instrumentation-only and excluded from the app APK.
- R-24 PASS: every menu route opens an existing editor, workspace, guide or Android document contract.
- R-25 PASS for shipped palette: checker measured Text #F4F5F6 / Panel #1C2024 = 15.01:1, Muted #B1BAC4 / Panel = 8.34:1, Accent #FFBD69 / Panel = 9.94:1. Selected amber #FFBD69 / #44351F = 7.18:1. Outline #78838E / Panel = 4.24:1 meets the non-text 3:1 bar; outline is not used as normal text. Text over thumbnails is avoided.
- R-26 PASS (source wiring): no no-op tool controls. Availability requirements produce disabled states or explicit empty panels. Implemented and unavailable feature groups are text, not fake controls.
- R-27 PASS: Home has empty/loading/error/retry; graph and tracking have prerequisite/empty states; media operations expose busy/progress/cancel and recoverable error dialogs; failed autosave shows an error.
- R-28 PASS: guide contains actual workflows and feature limits; no generic FAQ.
- R-32 PASS for observed keyboard navigation: Tab focuses the Home info control; Enter opens the guide; Escape closes it. The guide back control shows a visible focus state in keyboard-enter.png. Custom graph/preview drags have labeled Material sliders as alternatives. A complete TalkBack traversal remains unverified.
- R-33 PASS: app changes live in Kotlin/Compose source; no runtime source/style patch injection.
- R-34 PASS: one explicit dark creative-workspace theme. No broken theme toggle is shipped.
- R-35 PASS for the recorded workflow scope: native build, both core UI journeys, renderer/media/encoder tests, keyboard navigation and responsive screenshots are recorded. Source review connects remaining controls to editor mutations or Android document contracts; there are no dead placeholder tools. Every provider/error/gesture combination is not exhaustively exercised.
- R-36 PASS: no fabricated compatibility, performance, security or full-AE/Blender claims. Guide lists unavailable features.
- R-37 PASS: DESIGN.md preserves the user-approved AE/Alight-inspired charcoal/amber workspace; ENERGY 1 / RHYTHM 2 / MOTION 2.
- R-38 PASS: examples are labeled, project counts/thumbnails are actual, and face tracking uses a real detector.
- R-01/R-04/R-06/R-07/R-08/R-09/R-10/R-12/R-13/R-14/R-19/R-22 PASS: no decorative gradients/glow/glass/illustrations/badges. Material symbols name actual editing tools. Monospace aligns numbers; graph and studio grids encode data/spatial reference. Geometry is editable scene content. Reasons are in DESIGN.md.
- Liveliness PASS: Home emphasizes creation and actual projects; editor emphasizes composition; graphs emphasize the selected curve. Amber consistently marks selection. Whitespace separates transport, tracks and controls. No decorative animation competes with the composition.
- C-1/C-2/C-3/C-5 PASS: layout purposes are documented; tool routes are wired; content follows actual editing tasks; claims match implemented scope.
- C-4 PASS for observed states: tests cover empty project, stored projects, activity recreation, advanced-workspace prerequisites, media processing and responsive layouts. Busy/error/cancel paths exist in source. This is not a blanket resilience claim across every device/provider, low-memory state or assistive input.
- R-05/R-11/R-15/R-16/R-20/R-21/R-29/R-30/R-31 PASS (source/design review): task-specific workspaces, concrete labels, restrained corners, one global accent and no marketing filler. Dark mode comes from the requested creative editor direction. Comments explain only constraints such as source-time and platform behavior.
- Human supplement PASS for observed checks: palette contrast is computed, gestures have native slider alternatives, 200% Home/guide reflow is inspected and a Tab/Enter/Escape route is demonstrated. Complete TalkBack, every IME and 200% font in every panel remain unverified.
- Code-comment supplement PASS: no decorative comment banners, step narration, emoji or empty TODOs were added. Functional code was implemented under the user’s request; antislop-code was applied only to comment quality.

## Limits that matter when evaluating the build

This is an initial motion editor with working advanced tools, not full After Effects or Blender parity. Studio uses painter-sorted polygons with simple lighting, not a general depth-buffered scene renderer; OBJ imports geometry, not textures/materials. Camera settings are static; object transforms can be animated. Bézier timing is shared across a layer’s pose components, not independent temporal handles per property. Time remap uses nearest decoded video frames, with no audio or optical flow. Face tracking estimates image position/scale and head Euler angles at 5 fps, not calibrated 3D depth, face mesh, live AR or scene camera solve.

All-OEM codecs, physical-device performance, arbitrary face videos, document providers, low-memory behavior, process death during an open document picker, and cloud task restoration are unverified. Activity recreation/autosave is covered by the UI test. Export/tracking use the activity scope and can be cancelled by activity destruction. ML Kit adds Internet/network-state permissions; no claim is made that SDK telemetry is absent.

## Rotation investigation

A manual sequence of dialog dismissal, inspection and rotation produced a focus-event timeout in software emulation. The ANR reason was `Waited 5007ms for FocusEvent(hasFocus=false)`; the captured app main thread was idle in MessageQueue.nativePollOnce and the render thread was waiting for work. This snapshot does not prove a renderer deadlock or a sole emulator cause. The Activity now handles orientation/screen-size changes without destruction so editing, selection and active work survive rotation. The device regression checks portrait→landscape→portrait with the same Activity and project. Android CLI inspection is kept out of concurrent instrumentation runs.

The Android 11 System UI clock clips at 200% font in this emulator. Home and guide content reflow correctly; app screenshots retain the system clock artifact rather than hiding it.

Final APK SHA-256: `2807ca1240cbb5da3d6cf7464657c47eec16bf39bd1d48ec1fec8a41177d1750`. Final APK size: 56761975 bytes.
