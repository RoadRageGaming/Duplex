Duplex
A native, console-inspired game library and launcher for Windows and macOS.

Duplex brings your game collection, player profile, visual customization, and local-network tools into one desktop interface. The project provides separate native editions for Windows and macOS; some platform-specific features differ because each operating system exposes different APIs.
Current release: 1.0.0 Release

Duplex is an unsigned, community-style release. The Windows build is not digitally signed by Microsoft, and the Mac app is not notarized by Apple. Review the source and download packages only from the project's official GitHub Releases page.

Downloads
Get the ready-to-run packages from GitHub Releases. Open the release and choose the asset for your operating system.
Platform	Package	Notes
Windows, 64-bit	Duplex for Windows 1.0.0 Release.zip	Extract the ZIP, then run the included .exe.
macOS 14 or later	Duplex for Mac 1.0.0 Release.zip	Extract the ZIP completely; keep Duplex.app and Open Duplex.command together.
Source	This repository	Source folders and build notes are included for both editions.


If you're looking for an older version, browse the release history.
Features
Shared features
- Game library: Organize games and launchers in a single collection.
- Player profile and appearance: Customize your player profile, accent theme, and library presentation.
- Artwork and history: Add custom cover artwork and view recently played / launch history information.
- Library backup and restore: Export library information and preferences to a JSON backup, then restore it later with confirmation.
- Check Game Paths: Find library entries whose saved paths are missing or have moved. The check does not automatically delete those entries.
- Global search: Focus game search with Ctrl+K on Windows or Command+K on Mac.
- Local-network tools: Inspect available Wi-Fi networks and run connection diagnostics, subject to each operating system's permissions and capabilities.
Windows edition
- Native Win32 desktop interface with mouse and keyboard input, plus compatible XInput controller navigation.
- Network center with Wi-Fi scanning and connection diagnostics.
- Windows Mobile Hotspot controls, where supported by Windows and the device's network hardware.
- Game Drop: Receive Windows .exe launcher files over a local network through a PIN-protected upload page. Uploaded files are checked for an executable signature and added to the library; Duplex does not launch them automatically.
Mac edition
- Native SwiftUI desktop interface for macOS, built as a universal launcher for Apple Silicon and Intel Macs.
- Network center with nearby Wi-Fi scanning and diagnostics for router reachability, outbound connectivity, and DNS.
- Game Drop: Receive .zip app packages and .dmg disk images through a PIN-protected local upload page. Uploaded packages are saved locally and are not installed or launched automatically.
Installation
Windows
1. Open GitHub Releases and download the Windows ZIP package.
2. Extract the ZIP to a folder you control.
3. Run Duplex for Windows 1.0.0 Release.exe.
4. If Windows displays SmartScreen or another security warning, confirm that the file came from the intended project release before deciding whether to run it. The current build is unsigned.
macOS
1. Open GitHub Releases and download the Mac ZIP package.
2. Extract the ZIP completely. Do not run the app directly from inside the ZIP preview.
3. Keep Duplex.app and Open Duplex.command in the same extracted folder.
4. If macOS blocks the command file, Control-click Open Duplex.command and select Open, then follow the prompts.
5. The installer places the app in ~/Applications/Duplex.app and may ask to replace an earlier copy.
The Mac launcher compiles its SwiftUI interface on first launch using Apple's Swift compiler. Xcode Command Line Tools may be required. This release is ad-hoc signed for packaging but is not Apple-notarized.
Using Game Drop
Game Drop transfers a package between devices on a reachable local network. It does not require the devices to use the internet, but they must be able to reach one another on the network.
1. Connect the sending device and the computer running Duplex to the same local network.
2. Open Game Drop in Duplex and start the portal on the current network.
3. Use the displayed local address on the sending device; enter the session PIN shown by Duplex when prompted.
4. Upload the file type supported by that edition. The current upload limit is 100 MiB per file.
5. Return to Duplex and use the incoming-packages controls to reveal the uploaded file and review it before opening it.
Supported upload types differ by platform: Windows accepts .exe launcher files; Mac accepts .zip app packages and .dmg disk images. Uploaded content is not run automatically. A file extension or executable signature is not a guarantee that a file is safe—only transfer files from sources you trust.
About creating a Wi-Fi hotspot
Duplex can expose the network controls supported by its operating system, but it cannot silently configure every kind of hotspot. On Windows, hotspot support depends on Windows settings and the computer's network hardware. On macOS, the actual hotspot name, password, and Internet Sharing configuration are managed by macOS; Duplex does not create a Wi-Fi hotspot by itself. If the Mac and phone can already reach each other over the same Wi-Fi, use Start on Current Network instead of creating a separate network.
Backups and local data
Duplex stores its library and settings locally. The JSON backup can include profile preferences, game names, saved paths, artwork paths, and launch-history metadata. It does not copy game executables or artwork files themselves. You may need to correct paths if files have moved or a backup was restored on a different operating system.

Backups may reveal information about local folders and preferences. Store them somewhere private and restore only backup files you trust. Restoring replaces the current Duplex library/settings after confirmation.

Known locations:
- Windows library/settings: %APPDATA%\Duplex\library.json
- Windows diagnostic log: %APPDATA%\Duplex\Duplex-Hotspot.log
- Mac launcher log: ~/Library/Logs/Duplex/launch.log
- Mac Game Drop log: ~/Library/Logs/Duplex/portal.log
Wi-Fi scanning on macOS may require Location Services permission. Network diagnostics and Game Drop may also trigger the Local Network permission prompt. Some probes can be blocked by a router, firewall, VPN, or network policy.
Keyboard shortcuts
Action	Windows	Mac
Focus global game search	Ctrl+K	Command+K
Add a game	See the in-app controls	Command+A
Open the guide	See the in-app controls	G


The available shortcuts and navigation controls are platform-specific; consult the in-app guide for the current edition.
Build from source
The repository includes separate source directories for the Windows and Mac editions. Use the matching platform's source and instructions; one edition's source is not a substitute for the other.
Windows
Requirements:
- Go 1.23 or newer
- Python 3 for the optional embedded-icon step
- A Windows development environment, or a Go environment that supports Windows cross-compilation
From the Windows source directory, build the executable with:
go build -trimpath -ldflags="-H=windowsgui -s -w" -o "Duplex for Windows 1.0.0 Release.exe" main_windows.go
python embed_icon.py "Duplex for Windows 1.0.0 Release.exe" Duplex.ico
The icon-embedding step updates the executable's Windows icon resource.
macOS
Requirements:
- macOS 14 or later for the app
- Go 1.23 or newer
- Xcode Command Line Tools, including lipo, codesign, and Apple's Swift compiler
From the Mac source directory, run:
chmod +x build-macos.sh
./build-macos.sh
The build script produces a universal app bundle for Apple Silicon and Intel. The launcher compiles the SwiftUI interface at runtime, so a successful package build is not a substitute for testing first launch on a real Mac.
Troubleshooting
- Game Drop says it is not running: Start it again and check the portal status and log. Confirm that macOS/Windows firewall settings allow local-network communication and that the devices are on the same reachable network.
- Phone cannot open the portal: Use the exact address shown by Duplex, not localhost. Verify both devices are on the same network and that client isolation is disabled on the router or guest network.
- Wi-Fi scan returns no networks on Mac: Review System Settings → Privacy & Security → Location Services and allow Duplex if prompted. Reopen the Network Center and scan again.
- A game no longer launches: Use Check Game Paths and update the saved path if the game was moved.
- Mac app does not launch: Extract the ZIP fully, keep the app and command file together, check the launcher log, and install Xcode Command Line Tools if Swift compilation is unavailable.
- A backup won't restore: Use a valid JSON backup created by Duplex. Review the restore confirmation carefully; restore replaces the current library/settings.
Reporting a bug
Open a GitHub Issue and include:
- Which edition and version you are using (Duplex for Windows or Duplex for Mac, version 1.0.0 Release).
- Your operating-system version and, if relevant, your Mac chip type or network adapter.
- The steps that reproduce the problem and what you expected to happen.
- The relevant diagnostic log excerpt, with private paths, usernames, PINs, and other sensitive details removed.
Do not attach game files, passwords, portal PINs, or private backup JSON files to public issues.
Contributing
Bug reports and focused pull requests are welcome. Before submitting a change, describe the user-visible behavior it affects and test it on the target operating system when possible. For cross-platform changes, document any Windows/macOS differences rather than assuming both systems expose the same capabilities.
License
No LICENSE file is currently included with this README. Until the repository owner adds a license, you should not assume permission to reuse, modify, or redistribute this project's code. Add the license you intend to use to the repository before inviting outside contributions under specific licensing terms.
