# FreeTube TV prototype

This is a personal Android TV interface prototype derived from
[SmartTube](https://github.com/yuliskov/SmartTube). Its display name is **FreeTube**
and its independent Android application ID is `app.freetube.tv.prototype`.
It is not affiliated with YouTube, Google, or the existing FreeTube desktop project.

## Scope

The interface changes provide a dark TV browsing screen, a readable navigation
rail, and remote-focus styling using SmartTube's native TV views. They do not copy
the proprietary YouTube APK or its assets. SmartTube's playback, account selection,
subscriptions, history browsing, and optional SponsorBlock paths remain the basis
of the app. No ReVanced patches are embedded.

The separate package installs alongside official YouTube and SmartTube. It has its
own app data and requires its own sign-in. History and subscriptions depend on the
upstream service implementation and your account; an interface change cannot
promise parity with official YouTube. Playback and ad handling depend on upstream
changes. SponsorBlock only skips segments available in its community database.

The prototype has no upstream APK update feed. “Check for updates” explains that
updates must come from the same builder. Retain the signing key to update the app
without removing its data. SmartTube's external voice-search bridge may recognize
only official package IDs; device integration is not claimed for this prototype.

## Source and licenses

The inspected SmartTube base commit is
`380a2b79d3aa1af44a7b8ffbb406ae483a13fbd6`.
The pinned top-level submodules are:

- MediaServiceCore: `05feeb7e7889ab0243e9d44cb06bc8a7d3f066b6`
- SharedModules: `799cfae8a2f5c609b99e87fc673fc62de1c0199a`

SmartTube, MediaServiceCore and SharedModules carry MIT licenses. Keep their
copyright/license notices. Included libraries retain their own notices, including
Apache-licensed components and the EPL-1.0 notice for bundled j2v8. The repository's
LICENSE files are authoritative; do not describe every dependency as MIT.

## Build

Initialize the pinned dependencies after obtaining this prototype's source:

```sh
git submodule update --init --recursive
```

Use JDK 17, Android SDK platform 34, and the SDK build tools/NDK versions specified
by the Gradle files. Set `JAVA_HOME` and `ANDROID_HOME` for your machine. Use the
checked-in Gradle wrapper. The new flavor is `stfreetube`.

```sh
bash gradlew :smarttubetv:assembleStfreetubeDebug --no-daemon --max-workers=2
```

On Windows, use `gradlew.bat` instead of `bash gradlew`. Debug is suitable for a
private prototype and is signed with the machine's development key. Select the
`armeabi-v7a` APK for the projector whose Android userspace reports 32-bit ARM,
even if the underlying CPU supports 64-bit operation. APK outputs are under
`smarttubetv/build/outputs/apk/stfreetube/debug/`.

Relevant checks, whose actual outcome must be reported by the builder:

```sh
bash gradlew :smarttubetv:lintStfreetubeDebug :common:testStfreetubeDebugUnitTest --no-daemon --max-workers=2
```

Do not treat a successful compilation, syntax check, or a zero-test run as evidence
that sign-in, streaming, ad handling, or remote navigation works on a TV.

## Install and verify

Use ADB on the laptop that can reach the projector. Determine its current debugging
connection IP and port; pairing and connection ports differ. Then, substituting the
actual serial and APK path:

```sh
adb -s DEVICE_SERIAL install -r PATH_TO_ARMV7_APK
adb -s DEVICE_SERIAL shell monkey -p app.freetube.tv.prototype -c android.intent.category.LEANBACK_LAUNCHER 1
```

This is a media application. Do not set it as Android Home, replace system YouTube,
change DNS, or modify the projector's remote mapping merely to test it.

Validate on the real Android 14 projector:

- App tile and name are distinct; official YouTube and SmartTube still launch.
- D-pad can reach every navigation entry, video row, search control, and Settings;
  Back returns predictably and focus stays visible.
- Anonymous browsing and video/audio playback work on more than one video.
- Account sign-in, subscriptions and watch history work if the user opts to sign in.
- SponsorBlock settings are available; test a video with known submitted segments.
- Player seek, pause, subtitles and quality controls work with the physical remote.
- Home, Settings and held-volume behavior match the projector's existing setup.
- Process restart and device reboot preserve the app's settings.
- Check for updates reports manual updates instead of installing upstream SmartTube.

Report unsupported or untested behavior explicitly. This document describes the
procedure, not a claim that device tests have been performed.
