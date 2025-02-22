# SVGOMG / Trusted Web Activity

This project uses the
[Trusted Web Activities](https://developers.google.com/web/updates/2017/10/using-twa) technology
to wrap [SVGOMG](https://jakearchibald.github.io/svgomg/) in an Android Application.

> :warning: **Important:** This demo is still being maintained, but is now automatically 
generated via [llama-pack](https://www.npmjs.com/package/@llama-pack/cli). We **strongly recommend**
developers who want to bootstrap their Trusted Web Activity project to use
[llama-pack]([llama-pack](https://www.npmjs.com/package/@llama-pack/cli)). Since the content
in this repository is now automatically generated, we'll also be closing the issue tracker. Issues
should be filed directly in [bugs.chromium.org](https://bugs.chromium.org/), using [this template](https://bugs.chromium.org/p/chromium/issues/entry?components=UI%3EBrowser%3EMobile%3ETrustedWebActivities).

If you have a Trusted Web Activity related question, the best place to ask it is on StackOverflow, on the
[trusted-web-activity tag](https://stackoverflow.com/questions/tagged/trusted-web-activity), which is also
monitored by the team.

## Running the Demo

1. Clone the project
``
git clone https://github.com/GoogleChromeLabs/svgomg-twa.git
``

2. Import the Project into Android Studio, using File > New > Import Project, and select the folder
to which the project was cloned.

3. Run the Project (Ctrl+R)

### Enabling Debug

TWAs require [Digital AssetLinks](https://developers.google.com/digital-asset-links/) to be setup
on both the application and on the website, in order to enable the validation that allows Chrome to
open the page in full-screen.

For security reasons, the signing key compatible with the setup on
https://svgomg.firebaseapp.com/ is not committed with the sample code.

It is possible to setup Chrome to skip validation on device to enable testing.

Here are the 2 steps required to achieve this:

1. Enable Chrome to accept command-line parameters:

On the Android Device, go to the Chrome version being used to test the TWA and navigate to
`chrome://flags`. Search for a setting called `Enable command line on non-rooted devices` and
change it to `Enabled`. Restarting the browser *multiple* times may be required.

2. Create an Android file with the command-line parameters that allow skipping the TWA validation.

Add a file at `/data/local/tmp/chrome-command-line`, with the content
`_ --disable-digital-asset-link-verification-for-url="https://svgomg.firebaseapp.com"`. Make sure
there's not newline at the end of the line, or it may break the launcher.

For convenience, a shell script that creates this file is available in this repository. Run it
by executing `./enable-debug.sh https://svgomg.firebaseapp.com`.

To debug a different PWA, execute the script with a different host:
`./enable-debug.sh https://example.com`

### Debugging Digital Asset Links

As the debug certificate is different from the release one, and the fingerprint for debug should not be listed on the assetlinks.json file, is important to check if your Digital Asset Link is linked and verified.

After you generated your [signed APK](https://developer.android.com/studio/publish/app-signing#sign-apk). it can be installed into a test device, using adb:

``
adb install app-release.apk
``

If the verification step fails it is possible to check for error messages using the Android Debug Bridge, from your OS’s terminal and with the test device connected.

``
adb logcat | grep -e OriginVerifier -e digital_asset_links
``

If it is failing you'll see ``Statement failure matching fingerprint. Verification failed.`` message. Therefore is important to *review* *AndroidManifest.xml* and *build.gradle* files and check if the configurations are matching with the *assetlinks.json*.

Otherwise ``Verification succeeded.`` message should appear.

## License

```
Copyright 2015 Google, Inc.

Licensed to the Apache Software Foundation (ASF) under one or more contributor
license agreements. See the NOTICE file distributed with this work for
additional information regarding copyright ownership. The ASF licenses this
file to you under the Apache License, Version 2.0 (the "License"); you may not
use this file except in compliance with the License. You may obtain a copy of
the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the
License for the specific language governing permissions and limitations under
the License.
```
