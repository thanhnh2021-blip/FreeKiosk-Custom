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
