# Clipper for iOS

> Clipper is a clipboard manager for iOS that can monitor your clipboard indefinitely in the background — no jailbreak required.

[![Swift Version](https://img.shields.io/badge/swift-5.0-orange.svg)](https://swift.org/)
[![License: Unlicense](https://img.shields.io/badge/license-Unlicense-blue.svg)](http://unlicense.org/)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)

Unlike other clipboard managers available in the App Store, Clipper is able to monitor your clipboard indefinitely in the background. This is accomplished through a combination of workarounds, none of which would pass App Store review. For that reason, Clipper is available to download through [SideStore](https://github.com/SideStore/SideStore) — or by compiling the source code yourself.

<p align="middle">
<img title="Clipper Main Screen" src="https://user-images.githubusercontent.com/705880/63391950-34286600-c37a-11e9-965f-832efe3da507.png" width="375"> 
</p>

<p></p>

## Features
- Runs silently in the background, monitoring clipboard changes and asking the user if they'd like to add anything copied to Clipper.
- Save text, URLs, and images copied to the clipboard.
- Copy, delete, and share clippings.
- Customizable history limit.

## Requirements
- Xcode 11
- iOS 13
- Swift 5+

## Project Overview

All things considered, Clipper is a very simple app. The core app target can be mentally divided up into UI and logic, while each additional target serves a specific role.

### App UI
The entire UI is implemented with just two view controllers:

**HistoryViewController**  
The main screen of Clip. A relatively straightforward `UITableViewController` subclass that fetches recent clippings from Clip’s persistent store and displays them in a table view.

**SettingsViewController**  
The settings screen for Clip. Another `UITableViewController` subclass that displays all Clipper settings in a list, but is presented as a popover due to limited number of settings.

### App Logic
The app logic for Clipper is relatively straightforward. Most is self-explanatory, but there are two classes that serve particularly important roles:

**PasteboardMonitor**  
As you might have guessed from the name, this class is in charge of listening for changes to the clipboard. Since `UIPasteboardChangedNotification` is only received when the app is in the foreground, this class uses the private `Pasteboard.framework` to start sending system-wide Darwin notifications whenever the clipboard’s contents change. Once a change is detected, PasteboardMonitor presents a local notification that can be expanded by the user to save their clipboard to Clip.

**ApplicationMonitor**  
This class manages the lifecycle of Clipper. Specifically, it is in charge of setting up the background location API (make sure you set this to "Always Allow") so Clipper can run indefinitely in the background, as well as presenting a local notification whenever Clipper stops running (as iOS may terminate it in certain random conditions).

### ClipKit
ClipKit is a shared framework that includes common code between Clipper, ClipboardReader, and ClipBoard. Notably, it contains all model + Core Data logic, so that Clipper and each app extension can access the same persistent store with all clippings.

### ClipboardReader
ClipboardReader is a Notification Content app extension used to read the clipboard while Clipper is running in the background. When Clipper detects a change to the clipboard, it will present a local notification. If this notification is expanded, ClipboardReader will be launched and save the contents of the clipboard to disk before dismissing the now-expanded notification.

<p align="middle">
<img title="ClipboardReader" src="https://user-images.githubusercontent.com/705880/84835557-b4dedf80-afe8-11ea-94c5-52731b3ac15c.gif"><br>
<em>ClipboardReader in action.</em>
</p>

### ClipBoard
ClipBoard is a Custom Keyboard app extension that provides quick access to your recent clippings when editing text. This feature is still being worked on, so it is only available in beta versions of Clipper for now.

### Roxas
Roxas is Riley Testut's internal framework, developed to simplify a variety of common tasks used in iOS development. For more info, check out his [Roxas repo](https://github.com/rileytestut/roxas).

## Compilation Instructions
Clipper is very straightforward to compile and run if you're already an iOS developer. To compile Clip:

1. Clone the repository 
	``` 
	https://github.com/lonkelle/Clip.git
	```
2. Update submodules: 
	```
	cd Clip 
	git submodule update --init --recursive
	```
3. Open `Clip.xcworkspace` and select the Clip project in the project navigator. On the `Signing & Capabilities` tab, change the team from `Yvette Testut` to your own account.
4. Build + run app! 🎉

## Licensing

Clipper uses no 3rd party dependencies and it's original creator, Riley Testut, decided on the "unlicense" as decribed [here](https://github.com/rileytestut/Clip/blob/master/UNLICENSE), but the gist is:

> Anyone is free to copy, modify, publish, use, compile, sell, or distribute this software, either in source code form or as a compiled binary, for any purpose, commercial or non-commercial, and by any means.

## Contact Me

* GH Profile: [@lonkelle](https://github.com/lonkelle)
