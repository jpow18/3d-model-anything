# Use an iPhone scanner from Linux or Omarchy

You can manage the scanner from Linux without owning a Mac. Your iPhone captures
the room; a manually started GitHub Actions job uses a macOS runner to compile
the app. Linux handles installation, exported scans, and the browser viewer.
This does not port Apple's RoomPlan or ARKit to Linux.

## Requirements

- A LiDAR-equipped iPhone or iPad running iOS/iPadOS 17 or newer. Having an iPhone
  alone does not guarantee LiDAR support; the app checks support before capture.
- A USB data cable, an Apple account for signing on your own computer, and a
  GitHub account with access to this fork's Actions.
- [iloader](https://iloader.app/), an existing open-source Linux IPA installer.
  It offers an AppImage and a community AUR package. Install its `usbmuxd`
  prerequisite using your distro's package manager. No Apple account or signing
  secrets belong in this repository or its GitHub Actions settings.

## 1. Get the iPhone app

Open this fork's **Actions → Build iPhone app → Run workflow** and select `main`.
If GitHub asks you to enable workflows in your fork, enable them first. The build
is manual and does not submit anything to Apple or deploy a website.

When the job succeeds, download **iphone-app-unsigned** from its Artifacts section
and extract the ZIP. Inside is `3D-Model-Anything-unsigned.ipa`. This is a physical
iPhone build, not a simulator app. Artifacts expire after 14 days; you can run a
new build when needed. Public-repository standard hosted runners are currently
free; check your GitHub plan before choosing other runner types or visibility.

## 2. Install from Linux

1. Connect and unlock the iPhone. Accept **Trust This Computer** when asked.
2. Launch iloader, select your phone, and choose its option to install a custom
   IPA. Select `3D-Model-Anything-unsigned.ipa`.
3. Sign in to Apple through iloader locally and complete its signing/install
   prompts. The downloaded IPA is unsigned; copying it to the phone or using
   `ideviceinstaller` alone will not make it launch.
4. If prompted, trust the developer under **Settings → General → VPN & Device
   Management**, then enable **Settings → Privacy & Security → Developer Mode**
   and follow the restart prompts.
5. Open **3D Model Anything** and allow camera access.

Free Apple-account signatures normally expire after seven days; refresh or
re-sign through your sideloading tool before expiry. Re-sign with the same
account/app identity to preserve access to existing app data. Export important
scans before uninstalling the app. iloader handles Apple credentials, so get it
only from its [official site](https://iloader.app/) or linked repository.

## 3. Scan and transfer to Linux

Tap **Scan a room**, move around the room to improve coverage, then finish and
save it. Open the saved room and choose **Share room (.zip)**. Save it to Files
and transfer it to Linux with a file-transfer method you already use. AirDrop is
not required. The export retains the existing `house-capture/5` format: layout,
LiDAR mesh, photos with camera poses/calibration, and depth frames.

Keep the export private. Extract it under the ignored `data/captures-in/` directory
and follow [the reconstruction instructions](../README.md#use-the-skill).
The existing Node.js viewer and Python import tools run on Linux; capturing a
room does not automatically reconstruct its detailed editable furniture.

## Verification limits

The build job checks compilation and IPA packaging. It cannot check USB pairing,
Apple-account signing, LiDAR capture, or export on your physical phone. Those
steps need a real-device check. This fork does not add Morada scan import yet.
