# FreeTube TV prototype

Private-use Android TV prototype derived from SmartTube, independently named FreeTube. Not affiliated with Google, YouTube, SmartTube's maintainers, or the FreeTube desktop project.

[Download 32-bit ARM APK](https://github.com/98hallifer-wq/SmartTube/raw/refs/heads/freetube-tv-prototype/freetube-prototype/FreeTube-TV-prototype-armv7.apk)

Package: `app.freetube.tv.prototype`. Version: `32.68-freetube.1`. Minimum Android 8, target Android 14. Installs alongside existing apps. Signed with a development key; not a production release. Keep the same signing key for future updates.

Build passed; 11 unit tests passed; lint: 0 errors, 272 warnings. Physical TV tests, streaming, account history, and SponsorBlock behavior remain unverified. Preserve original apps and device settings. See INSTALL.txt, FREETUBE.md, BUILD-RESULTS.txt and SHA256SUMS.txt.

## Reproduce the source

The prototype's source changes are in source-changes.patch. Apply to this exact upstream commit, not the default branch of this fork:

```sh
git clone https://github.com/yuliskov/SmartTube.git FreeTube-TV
cd FreeTube-TV
git checkout 380a2b79d3aa1af44a7b8ffbb406ae483a13fbd6
git submodule update --init --recursive
git apply /path/to/source-changes.patch
```

Then build with the instructions in FREETUBE.md, using JDK 17 and the Android SDK. Dependencies and license notices remain required; notices are included under licenses/. No proprietary YouTube APK or its assets are incorporated.
