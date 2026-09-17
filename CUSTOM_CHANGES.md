# FreeKiosk Custom – build-once configuration

This build is intended to be built once and then configured from FreeKiosk Settings.

## Changes

1. Multi-App Home background is now selectable from Settings → Display → Multi-App Home Background.
   - Choose JPG/PNG from Android picker.
   - Image is stored in app-private `multiapp_background` storage.
   - No APK rebuild is needed when changing the wallpaper later.
   - Restore Built-in Background removes the selected image and uses the bundled `multiapp_background.jpg`.

2. Multi-App Home supports native tap-to-settings.
   - Uses the existing Security → Return to Settings settings.
   - `Tap Anywhere` + configurable tap count/timeout works on the Multi-App launcher, including over FlatList/app tiles.
   - Volume Up ×5 remains available as the fallback shortcut.

3. The selected background path is included in JSON configuration export/import.
   - The backup stores the path/configuration, not the binary image itself. Reinstalling the APK clears app-private files, so after a full uninstall/reinstall the image must be selected again.

## Build

The existing `.github/workflows/build-apk.yml` builds the release APK as the `FreeKiosk-custom-release` artifact.

4. Lifecycle hardening for return-to-settings gestures.
   - The upstream #203 OverlayService/MainActivity race fix is already present in this source:
     the native external-app fast path does not emit `onAppReturned` before restarting the OverlayService.
   - On every `MainActivity.onResume()`, transient 8-tap and Volume-Up x5 gesture state is reset.
   - Persisted AsyncStorage settings and Device Owner state are not changed.

5. Multi-App Home double-tap lock.
   - Double-tap an empty Home background area to invoke `KioskModule.turnScreenOff()`.
   - On this Device Owner tablet, the native method uses `DevicePolicyManager.lockNow()` for a real screen lock.
   - The lock target is layered below the header, app tiles, FlatList children, and return button, so normal app clicks remain unchanged.
   - Lenovo/ZUI double-tap-to-wake is untouched.
