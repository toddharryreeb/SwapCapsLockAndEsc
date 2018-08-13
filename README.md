# SwapCapsLockAndEsc
Swap CapsLock and Esc on a standard en-US keyboard in Windows 10.

## Why?
I swapped these keys because I use Vi keybindings whenever possible, but this
.reg file may be easily modified to swap CapsLock and LCtrl for Emacs
keybindings, for example.

## How to use
Modify the .reg file as desired (see below), double click it to execute, and
reboot. The changes will persist until the registry entry `Scancode Map` in 
`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Keyboard Layout` is 
deleted.

## Explanation
Making such a change to the keybindings amounts to choosing the right hex value
for Scancode Map, which is determined as follows:

Octuple | Purpose
--- | ---
00,00,00,00 | Header Version (fixed)
00,00,00,00 | Header Flags (fixed)
__,00,00,00 | Number of octuples (including the terminating NULL) that follow this one.
\_\_,\_\_,\_\_,\_\_ | Map entry that maps a scancode (first duple) to a key (second duple)
... | More map entries
00,00,00,00 | Terminating NULL (fixed)

For example, the value of `Scancode Map` in `SwapCapsLockAndEsc.reg`
```
hex:00,00,00,00,00,00,00,00,03,00,00,00,3A,00,01,00,01,00,3A,00,00,00,00,00
```
has three entries (hence 03,00,00,00):

Octuple | Purpose
--- | ---
3A,00,01,00 | Map scancode CapsLock (3A,00) to key Esc (01,00)
01,00,3A,00 | Map scancode Esc (01,00) to key CapsLock (3A,00)
00,00,00,00 | Terminating NULL

Using
```
hex:00,00,00,00,00,00,00,00,02,00,00,00,01,00,3A,00,00,00,00,00
```
instead would just turn CapsLock into another Esc key without changing Esc,
for example.

## Further reading
- [Reference for keyboard scancodes](https://www.win.tue.nl/~aeb/linux/kbd/scancodes.html)
- [Basic reference for writing .reg files](https://support.microsoft.com/en-us/help/310516/how-to-add-modify-or-delete-registry-subkeys-and-values-by-using-a-reg)
