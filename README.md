# KOReader SSH Client

A KOReader plugin that lets you connect **from your Kindle or other KOReader device to another computer over SSH**. Save host profiles, run remote commands, and open an interactive shell without leaving KOReader.

> **Important: This project is vibe-coded.** It was developed with AI assistance and has not been exhaustively reviewed or tested on every device and KOReader configuration. Expect bugs, review the code before use, and test it carefully before relying on it—especially in security-sensitive environments.

> **This is an SSH client, not an SSH server.** It does not open an incoming port or allow other devices to SSH into the Kindle.

## Features

- **Saved host profiles** with a friendly name, hostname or IP address, username, port, and optional private-key path.
- **Connection test** to check that a host can be reached and authenticated.
- **Remote command runner** for one-off commands.
- **Custom colour terminal (experimental)** with its own pseudo-terminal, ANSI colour rendering, scrollback, cursor movement, screen clearing, scrolling regions, and alternate-screen support.
- **Compact SSH keyboard** docked below the custom terminal. It includes QWERTY and symbol layouts, Shift/Ctrl modifiers, shell punctuation, Tab, Esc, Ctrl shortcuts, arrow keys, Home/End, Page Up/Down, Backspace, Space, and Enter. It does not open KOReader's normal virtual keyboard for the custom terminal.
- **Terminal resizing** when the keyboard is opened or closed, so interactive programs can adapt to the available screen area.
- **Built-in KOReader terminal fallback** for sessions that do not behave correctly in the custom terminal.
- Automatic discovery of a usable `dbclient` (Dropbear) or `ssh` executable, with an option to specify a custom executable path.

## Compatibility and current limitations

The plugin is designed for KOReader, including Kindle devices and noframework setups, but the available SSH executable and pseudo-terminal support depend on the device's environment.

The custom terminal is an early terminal emulator, **not a complete xterm replacement**. Some uncommon escape sequences, full-screen terminal applications, character-width combinations, and application-specific terminal behaviours may not render exactly as they do on a desktop terminal.

**Light/dark mode support is still being refined.** Depending on the KOReader build and device, parts of the custom terminal may not match the active theme or ANSI colours may look incorrect. The built-in KOReader terminal is available as a fallback from a host's action menu.

## Requirements

- KOReader installed on the device.
- An executable SSH client available to KOReader: preferably `dbclient` (Dropbear), or `ssh`.
- Network access from the device to the SSH host.
- For interactive custom-terminal sessions, a working pseudo-terminal (`/dev/ptmx`) on the device.

The plugin searches common KOReader and system locations for `dbclient` or `ssh`, then tries the system command path. If the executable is installed somewhere else, configure its full path in the plugin's settings/menu.

## Installation

1. Download or clone this repository.
2. Make sure the plugin files are inside a directory named **`sshclient.koplugin`**. That directory should contain `main.lua`, `_meta.lua`, and `ssh_color_terminal.lua`.
3. Copy `sshclient.koplugin` into KOReader's `plugins` directory. On many Kindle installations this is:

   ```text
   /mnt/us/koreader/plugins/sshclient.koplugin/
   ```

   The exact path can differ by device and installation.
4. Restart KOReader.
5. Open **Tools → More tools → SSH Client**.

If you cloned the repository and the repository directory is not named `sshclient.koplugin`, rename the directory or copy its contents into a directory with that name before installing it.

## First connection

1. Open **Tools → More tools → SSH Client**.
2. Choose **Add host**.
3. Enter a profile name (optional), hostname/IP address, remote username, SSH port (normally `22`), and an optional private-key file path.
4. Save the host profile and select it from the list.
5. Choose **Test connection** first. If it works, open **Open Color SSH Terminal (experimental)** or **Open built-in KOReader terminal (SSH)**.
6. To run a single command, choose **Run remote command**.

Host profiles are saved in KOReader's settings. The plugin saves the private-key **path**, not a copy of the private key itself. Key files should remain protected with appropriate filesystem permissions.

## Host-key security

Host-key verification is enabled by default. When connecting to a host for the first time, verify its fingerprint through a trusted channel before accepting it.

Each host profile has an optional setting to allow automatic first-use host-key acceptance. This is a convenience option that weakens the protection offered by the initial trust prompt; enable it only when you understand the trade-off. Do not automatically trust a host whose identity you cannot verify.

The plugin does not implement its own SSH cryptography. Authentication, encryption, and host-key handling are provided by the SSH executable that it launches.

## Choosing a terminal

### Open Color SSH Terminal (experimental)

Uses the plugin's custom ANSI renderer and docked keyboard. The terminal advertises `TERM=xterm-256color` and `COLORTERM=truecolor`, and supports common SGR colours including standard 16-colour, 256-colour, and truecolour sequences. Compatibility is not complete, especially for complex terminal applications.

### Open built-in KOReader terminal (SSH)

Runs SSH inside KOReader's own terminal emulator. Use this option if a remote program has rendering problems in the custom terminal or if the custom terminal's theme is not correct on your KOReader build. It requires KOReader's built-in **Terminal emulator** plugin to be enabled.

## Troubleshooting

- **SSH Client does not appear in the menu:** confirm that the directory is named `sshclient.koplugin`, that it is inside KOReader's `plugins` directory, and that KOReader was restarted.
- **No SSH executable found:** install or locate `dbclient`/`ssh`, then configure its full path through the plugin's settings/menu if needed.
- **Connection fails:** verify the hostname/IP address, port, username, network reachability, and authentication method. Try **Test connection** before starting an interactive terminal.
- **Interactive terminal will not start:** confirm that `/dev/ptmx` is available and that KOReader's terminal functionality works on the device. Try the built-in terminal option.
- **Colours or full-screen applications look wrong:** this can be a limitation of the custom terminal. Try the built-in KOReader terminal and include the command/application name and relevant KOReader log details when reporting a bug.
- **Keyboard or screen behaves oddly:** report your KOReader version, device model, whether noframework mode is used, the selected terminal option, and the steps needed to reproduce the problem.

## Reporting issues

When opening an issue, include:

- KOReader version and device model.
- Whether the device is running in noframework mode.
- Which terminal option you used.
- The command or application that triggered the issue.
- Relevant log output, with hostnames, usernames, addresses, and other private information removed.

Please do not post private keys, passwords, tokens, or unredacted connection details in bug reports.
