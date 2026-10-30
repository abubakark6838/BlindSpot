# Blindspot

Two files. The DLL goes on the PC, the APK goes on the phone.

| file | where it goes |
|---|---|
| `dbghelp.dll` | `C:\Program Files\BlueStacks_msi5\` (next to `HD-Player.exe`) |
| `panel.apk` | the phone |

## Install

1. Copy `dbghelp.dll` into the BlueStacks folder.
2. Install `panel.apk` on the phone.
3. Start the emulator. The overlay comes up about five seconds after its window appears.
4. In the panel: enter any licence key, then the PC's IPv4 address. Port is fixed at 47821.

`update.exe` does step 1 for you: it asks for administrator, finds every
BlueStacks install on the machine and drops the current DLL into each. Close
the emulator first — it holds the DLL open.

## Keybinds

The Aimbot and Enable ESP rows have a small key chip next to the switch. Tap
it, then press any key on the PC — that key becomes an instant on/off for that
feature.

- mouse side buttons work (shown as `Mouse4` / `Mouse5`)
- left, right and middle click are refused, because the aimbot holds LMB to fire
- one key drives one feature; binding a key that is already taken moves it
- the table is stored on the phone and replayed on every connect

## If the emulator will not start

The DLL is loaded by `HD-Player.exe` at process start, so a bad copy takes the
whole emulator down with it. Delete `dbghelp.dll` from the BlueStacks folder and
it starts again. Then re-run `update.exe` for a clean copy.

## USB instead of Wi-Fi

By default the DLL listens on this machine's LAN address, which is what the
panel expects: type the PC's IPv4 and go. That listener is visible to anything
that enumerates sockets, and it needs a firewall rule.

To keep it off the network entirely, put a `remote.cfg` next to the DLL with:

```
bind=loopback
```

The listener then exists only on `127.0.0.1`. Reach it over the cable, once per
session:

```
adb -s <phone-serial> reverse tcp:47821 tcp:47821
```

and point the panel at `127.0.0.1` instead of the PC's address. No firewall
rule, no LAN peer, nothing on the wire.

`port=<n>` in the same file moves the listener off 47821.

## Firewall

Only needed on the default LAN mode. If the phone cannot reach the PC, port
47821 is blocked. Once, in an admin prompt:

```
netsh advfirewall firewall add rule name="Blindspot" dir=in action=allow protocol=TCP localport=47821 profile=any
```

## Note on the cover name

`dbghelp.dll` is a name Windows already uses. The real one lives in
`System32`; this one sits beside the emulator, and Windows searches the
application folder first. BlueStacks ships no copy of its own, so this file is
the one that loads.
