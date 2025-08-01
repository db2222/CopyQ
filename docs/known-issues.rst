Known Issues
============

This document lists known commonly occurring issues and possible solutions.

.. _known-issue-window-tray-hidden:

On Windows, tray icon is hidden/repositioned after restart
----------------------------------------------------------

With current official builds of CopyQ, the tray icon position and hide/show
status are not restored after the application is restarted or after logging in.

**Workaround** is to use CopyQ binaries build with older Qt framework version (Qt
5.9); these are provided in latest comments in the issue link below.

.. seealso::

    `Issue #1258 <https://github.com/hluk/CopyQ/issues/1258>`__

.. _known-issue-windows-console-output:

On Windows, CopyQ does not print anything on console
----------------------------------------------------

On Windows, you may not see any output when executing CopyQ in a
console/terminal application (PowerShell or cmd).

**Workarounds:**

* Use different console application: Git Bash, Cygwin or similar.
* Use Action dialog in CopyQ (``F5`` shortcut) and set "Store standard output"
  to "text/plain" to save the output as new item in current tab.
* Append ``| Write-Output`` to commands in PowerShell:

  .. code-block:: powershell

    & 'C:\Program Files\CopyQ\copyq.exe' help | Write-Output

.. seealso::

    `Issue #349 <https://github.com/hluk/CopyQ/issues/349>`__

.. _known-issue-macos-paste-after-install:

On macOS, CopyQ won't paste after installation/update
-----------------------------------------------------

CopyQ is not signed app, you need to grant Accessibility again when it's
installed or updated.

**To fix this**, try following steps:

1. Go to System Preferences -> Security & Privacy -> Privacy -> Accessibility
   (or just search for "Allow apps to use Accessibility").
2. Click the unlock button.
3. Select CopyQ from the list and remove it (with the "-" button).

.. seealso::

    - `Issue #1030 <https://github.com/hluk/CopyQ/issues/1030>`__
    - `Issue #1245 <https://github.com/hluk/CopyQ/issues/1245>`__

.. _known-issue-wayland:

On Linux, global shortcuts, pasting or clipboard monitoring does not work
-------------------------------------------------------------------------

This can be caused by running CopyQ under a **Wayland** window manager instead
of the X11 server.

Depending on the desktop environment, these features may not be supported:

- global shortcuts
- clipboard monitoring
- pasting from CopyQ and issuing copy command to other apps (that is passing
  shortcuts to application)
- screenshot functionality
- retrieving and matching window titles
- querying keyboard modifiers and mouse position

**Workaround:** try using the **Wayland Support** command mentioned below or
set ``QT_QPA_PLATFORM`` environment variable to run the app under **Xwayland**
mode (additional package may be needed, for example:
``xorg-x11-server-Xwayland`` in Fedora).

For example, launch CopyQ with::

    env QT_QPA_PLATFORM=xcb copyq

If CopyQ autostarts, you can change ``Exec=...`` line in
``~/.config/autostart/copyq.desktop``::

    Exec=env QT_QPA_PLATFORM=xcb copyq

For **Flatpak** application, see `this workaround
<https://github.com/hluk/CopyQ/issues/2948#issuecomment-2614271330>`__.

.. note::

    Mouse selection will still work only if the source application itself
    supports it.

.. seealso::

    `Wayland Support
    <https://github.com/hluk/copyq-commands/tree/master/Scripts#wayland-support>`__
    command reimplements some features on Wayland through external tools (see
    `README <https://github.com/hluk/copyq-commands/blob/master/README.md>`__
    for details on how to add the command).

    `Issue #27 <https://github.com/hluk/CopyQ/issues/27>`__

Scripting command "copy()" fails
--------------------------------

The command ``copy()`` sends the Ctrl+C shortcut to the current window.
This can fail depending on the active application.
If CopyQ won't detect a clipboard change, it throws an exception.
The execution then fails with the message ``Failed to copy to clipboard!``.

An alternative under Windows is to use a Powershell script as a command:
  .. code-block:: powershell

    powershell:
      Add-Type -AssemblyName System.Windows.Forms;
      Start-Sleep -Milliseconds 300;
      [System.Windows.Forms.SendKeys]::SendWait("^c");
      Start-Sleep -Milliseconds 300;

The delay is added to make sure the text is copied to the clipboard.

Beware that necessary **permissions** for Powershell independent of CopyQ need to have been setup.