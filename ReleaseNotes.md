# Release notes

## v1.5.2 (21)
- Added field hints: a field with a hint shows an info button that opens the hint in a popup. `POPUP` fields and popup flows open their popup.
- Added back navigation: when a step has a `BACK` field, a back button is shown in the navigation bar. After a confirmation it opens the earlier step with the answers given there filled in (text, dates, checkboxes, dropdowns, radio buttons, signatures and files).
- Added `FBSAppearance.showBackButtonTitle` to show the `BACK` field's label next to the back arrow (default `false`).
- Fields without an earlier answer start with their `defaultValue`.
- Nested sections are now loaded. Section visibility supports the new `conditionGroup` format and the old `GROUP` conditions.

## v1.5.1 (20)
- Added iPhone Duo (foldable) support for KYC video: the front camera of the open half is used (outer Center Stage camera when folded, inner camera when unfolded) and switches automatically when the device is folded or unfolded.
- KYC video rotation is now computed by the system (`AVCaptureDevice.RotationCoordinator` on iOS 17+), keeping the preview and the streamed video in sync for every camera.
- The KYC camera view adapts when the screen is wider than tall (e.g. unfolded iPhone Duo, landscape).
- Camera switches now run off the main thread, fixing an app termination (watchdog) when switching cameras during a screen change.
- The SDK now sends device and camera information (resolution, camera, rotation) to the backend as a `debug` message at the start of each video stream, as the Android SDK does.
- Added release-build camera logs (`[FBSCamera]`) for troubleshooting.

## v1.5.0 (19)
- Added IBAN Verification step support (`IrisPayUI` integration).
- Added Single Image KYC (`SINGLE_IMAGE_KYC`) step support.
- Added audio recording support during KYC video streaming.
- Improved video streaming resolution selection, encoding bitrate, and WebSocket reconnection when switching cameras.
- Signature pad UI/UX improvements and file upload fixes.

## v1.4.8 (18)
- Refactored video streaming pipeline to use Fragmented MP4 (fMP4) via ISOBMFF.
- Consents bug fix

## v1.4.7 (17)
- Fix text color

## v1.4.6 (16)
- Fix url builder

## v1.4.5 (15)
- Added new delegate method `postToHostUI(status:)`.

## v1.4.4 (14)
- Consent checkbox now shows link if there is.
- New custom alert

## v1.4.3 (13)
- Links now open in an in-app WebView when the `inNewTab` property is set (instead of always external browser).

## v1.4.2 (12)
- Added new delegate method `processResult(success:)`.

## v1.4.1 (11)
- Fixed camera orientation issues when switching from back to front camera on iPhone 17.

## v1.4.0 (10)
- Added new types of calculators for extended functionality.
- Added support for displaying images in forms.
- Bugfixes.

## v1.3.3 (9)
- Improved communication with the backend.

## v1.3.2 (8)
- Significantly reduced the bit rate of the encoded video frames.
- Changed the keyframe encoding from every 60 frames to every 30 frames or every second (whichever comes first).

## v1.3.1 (7)
- Bugfixes.

## v1.3.0 (6)
- The backend instance URL is now mandatory when initialising the SDK.  

## v1.2.0 (5)
- Added ability to use your own FormBuilder/KYC instance.
- Bugfixes.

## v1.1.1 (4)
- Set an initial instructional text.

## v1.1.0 (3)
- KYC fixes.
- Added ability to hide the close button.
- Added ability to set custom button corner radii.
- Other appearance bugfixes.

## v1.0.1 (2)
- Backend updates.

## v1.0.0 (1)
- Initial release.
