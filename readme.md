# Appium

## What is Appium?

Appium is a software suite for automating testing of mobile applications. It is
developed by Sauce Labs.

[Appium website](https://appium.io)

## How does Appium work?

At the core of Appium is the Appium Server, which is a service which is able to
bridge the Node runtime where the Appium script is running with the automation
side where the emulated or attached application is being controlled.

Appium Desktop is a UI for Appium Server, allowing the user to start it up, shut
it down and configure its options.

Appium Inspector is a GUI for inspecting the component structure of a given
application, which assists during the automation script development process.

A library of the user's choice can be used to provider various other functions
on top of the Appium layer, such as assertions.

## Installation and initial setup

### [Appium Desktop](https://github.com/appium/appium-desktop)

Appium Desktop includes Appium Server.

Go to [the latest release of Appium Desktop][appium-desktop-latest].

[appium-desktop-latest]: https://github.com/appium/appium-desktop/releases/latest

Download whichever format of the file for macOS you prefer. In any case, on Big
Sur, permissions need to be adjusted to make the application runnable.

Find the Downloads directory in Finder and go up using Go > Enclosing Folder in
the Finder menu. Right-click on Downloads and select New Terminal at Folder.

In the Terminal window, type `xattr -cr Appium-Server-GUI-mac` and press Tab to
auto-complete the file name with the latest version in it. Run this command and
close Terminal. Now the downloaded file will be runnable. This is required as
macOS Big Sur uses Gatekeeper to check the signatures of downloaded applications
and the Appium Desktop app is not signed using a paid developer certificate.

More about this in [this Appium Desktop readme section][appium-desktop-macos].

[appium-desktop-macos]: https://github.com/appium/appium-desktop#installing-on-macos

Open and install Appium Desktop from the downloaded file. For DMG, this means
opening the DMG by double-click and dragging the Appium Desktop icon to the
Applications icon.

Once installed, Appium Desktop will be found in Applications and can be run and
pinned to the Dock.

Run Appium Desktop and click Start Server to start Appium Server:

![Appium Desktop screenshot](appium-desktop.png)

![Appium Server screenshot](appium-server.png)

### [Appium Inspector](https://github.com/appium/appium-inspector)

Go to [the latest release of Appium Inspector][appium-inspector-latest].

[appium-inspector-latest]: https://github.com/appium/appium-inspector/releases/latest

Download whichever format of the file and follow the same instructions as for
Appium Desktop above to make the file runnable on macOS Big Sur. Install Appium
Inspector in the same way and pin it to the Dock.

The combinations of versions of Appium Desktop and Appium Inspector that I am
using at the moment (Appium Desktop 1.22.0 and Appium Inspector 2021.9.2)
require that in Appium Inspector, the Remote Path field is set to `/wd/hub`
instead of the default `/` (which is coerced to `/session`). This is as per the
release notes of Appium Desktop which state:

> The version still expects to have /wd/hub prefix as Remote Path

Without this change, Appium Inspector will yield this error when clicking Start
Session:

> Failed to create session. The requested resource could not be found, …

The Appium Desktop UI will show this in the Appium Server logs:

```
[HTTP] No route found for /session
```

- [ ] Remove this information once Appium Desktop includes a version of Appium
  Server which no longer requires this

![Appium Inspector screenshot](appium-inspector.png)

- [ ] Update the Appium Inspector screenshot to use `/wd/hub` and then again to
  use the default (`/` placeholder) for Remote Path once the above is resolved

### First steps with Appium Inspector

Appium Inspector needs to have a set of capabilities provided to know what to do
once it connects to the Appium Server.

See the [capabilities documentation][caps] for the available capabilities.

[caps]: https://github.com/appium/appium/blob/master/docs/en/writing-running-appium/caps.md

A minimal set of capabilities for the iOS Simulator looks like this:

```json
{
  "platformName": "iOS",
  "platformVersion": "15.0",
  "deviceName": "iPhone XS Max",
  "app": "/path/to/my.app"
}
```

- `platformName` must be `iOS` for iOS and iOS Simulator
- `platformVersion` is the iOS version
- `deviceName` is as per `instruments -s devices`
  - `instruments` come with [XCode Command Line Developer Tools](xcode-clt)
  - `xcode-select --install` displays the current CLTs
- `app` is a local or remote path to an IPA (iOS device) or APP (iOS Simulator)
  file
  - See also `bundleId` for automating an existing app on iOS

[Here's how you can get the IPA or APP file from XCode][xcode-ipa-app]

[xcode-clt]: https://developer.apple.com/download/all
[xcode-ipa-app]: https://stackoverflow.com/q/1984727/2715716
