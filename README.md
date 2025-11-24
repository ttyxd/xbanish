## betterbanish

`betterbanish` is a fork of the original `xbanish` that aims to improve on the original by making kernel events mode the default for detecting input. This means you no longer need to specify the `-E` flag.

`betterbanish` hides the mouse cursor when you start typing, and shows it again when
the mouse cursor moves or a mouse button is pressed.
This is similar to xterm's `pointerMode` setting, but `betterbanish` works globally in
the X11 session.

unclutter's -keystroke mode is supposed to do this, but it's
[broken](https://bugs.launchpad.net/ubuntu/+source/unclutter/+bug/54148).

The name comes from
[ratpoison's](https://www.nongnu.org/ratpoison/)
"banish" command that sends the cursor to the corner of the screen.

### Implementation

`betterbanish` works by listening for keyboard and mouse events directly from the kernel's `evdev` interface (`/dev/input/event*`). This makes it independent of the X server's input handling and avoids the need for the XTest extension.

When a key is pressed, the cursor is hidden using the `XFixes` extension. When the mouse is moved or a button is pressed, the cursor is shown again.

`betterbanish` also uses `inotify` to monitor the `/dev/input` directory for new devices, so it will automatically detect and use newly connected keyboards and mice.

### Usage

```
usage: betterbanish [-a] [-c count] [-d] [-i mod] [-j pixels] [-m [w]nw|ne|sw|se|+/-xy] [-t seconds] [-s]
```

### Options

*   `-a`: Always keep mouse cursor hidden while `betterbanish` is running.
*   `-c count`: Hide the cursor after `count` keystrokes.
*   `-d`: Print debugging messages to stdout.
*   `-i mod`: Ignore pressed key if `mod` is used. Modifiers are: `shift`, `lock` (CapsLock), `control`, `mod1` (Alt or Meta), `mod2` (NumLock), `mod3` (Hyper), `mod4` (Super, Windows, or Command), `mod5` (ISO Level 3 Shift), and `all`.
*   `-j pixels`: Only show the mouse cursor again if it has moved more than `pixels` from where it was hidden.
*   `-m [w]nw|ne|sw|se|+/-xy`: When hiding the mouse cursor, move it to this corner of the screen or current window, then move it back when showing the cursor. Also accepts absolute positioning, for example `+50-100` will be positioned 50 pixels from the left and 100 pixels from the bottom.
*   `-t seconds`: Hide the mouse cursor after `seconds` have passed without mouse movement.
*   `-s`: Ignore scrolling events.