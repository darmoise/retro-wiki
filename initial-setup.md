# Initial Setup
This page describes a small first-day setup for the HTC Touch Diamond2, also known as HTC Topaz, running Windows Mobile 6.1/6.5/6.5.x.

The goal is to install Total Commander/CE, use it as a registry editor, and apply a few useful power/audio tweaks.

## 1. Download Total Commander/CE

For Windows Mobile, use **Total Commander for PocketPC / WinCE**, not the normal desktop Windows version:
- [2.51 (exe)](http://www.totalcommander.ch/tcce/251/tcmdpocketarm.exe) - 85dcc7ea4777ee996995f98bc299360605702ab73ce67484d15d4bdeb3e75043.
- [2.51 (cab)](http://www.totalcommander.ch/tcce/251/tcmdpocketarm.cab) - e5d9cf316867ad498b7aabb65ef5552340a061b0a3b4eea655a0f34e77094e2c.
- [2.52 beta 3 (exe)](https://www.totalcommander.ch/tcce/b3/touch/cecmd.arm.cab) - 75eae3934a3183670ac556b232972fda42ba67ed221fe71eea3b543803d35e0a.

## 2. Open the registry in Total Commander/CE

1. Start **Total Commander** on the device.
2. Go to the root of the filesystem:

   ```text
   \
   ```

3. Open:

   ```text
   Plugins
   ```

4. Open:

   ```text
   registry
   ```

The full path is:

```text
\\Plugins\registry
```

Inside the registry plugin, you should see registry hives such as:

```text
HKCR
HKCU
HKLM
HKU
```

Examples:

```text
\\Plugins\registry\HKLM
\\Plugins\registry\HKCU
```

## 3. Registry editing basics in Total Commander/CE

Total Commander displays registry keys like folders and registry values like files.

### Open a registry key

Open folders step by step, for example:

```text
\\Plugins\registry\HKCU\ControlPanel\BackLight
```

or:

```text
\\Plugins\registry\HKCU\ControlPanel\Keybd
```

### Edit an existing value

1. Open the registry key.
2. Double-tap the value.
3. Choose the correct base:

   ```text
   Decimal
   ```

   or:

   ```text
   Hexadecimal
   ```

4. Enter the new value.
5. Confirm with **OK**.

For values `0` and `1`, Decimal and Hexadecimal are the same.

Example:

```text
Base:  Decimal
Value: 1
```

### Add a new registry value

If a value does not exist, create it manually.

In Total Commander/CE, the last item in a registry key is usually:

```text
+Add value+
```

Steps:

1. Open the target registry key.
2. Scroll to the end of the list.
3. Double-tap:

   ```text
   +Add value+
   ```

4. Enter the value name.
5. Select the value type.
6. Enter the value data.
7. Confirm with **OK**.

For the tweaks below, the type is usually:

```text
DWORD
```

## 4. Keep music playing when the screen is off

### Goal

Music should continue playing when the screen is turned off with the Power button.

This means **screen off / standby**, not full device shutdown.

### Registry path

Open:

```text
\\Plugins\registry\HKLM\Software\HTC\AudioManager_Eng\Config
```

Find or create this value:

```text
Name:  enter_suspend
Type:  DWORD
Value: 0
Base:  Decimal
```

If the `AudioManager_Eng` path does not exist, also check:

```text
\\Plugins\registry\HKLM\Software\HTC\AudioManager\Config
```

and set:

```text
Name:  enter_suspend
Type:  DWORD
Value: 0
Base:  Decimal
```

On some HTC ROMs, this additional key may also exist or be useful:

```text
\\Plugins\registry\HKLM\Software\HTC\ScreenOffPlay
```

Set or create:

```text
Name:  Enable
Type:  DWORD
Value: 1
Base:  Decimal
```

### Test

1. Start music in HTC Music / HTC Audio Manager.
2. Press the Power button once.
3. The screen should turn off.
4. Music should continue playing.

If playback still stops, soft reset the device and test again.

---

## 5. Lock the device when the screen is turned off

### Goal

Pressing the Power button should turn off the screen and lock the device.

After waking the device again, the lock screen should appear.

### Enable lock on suspend

Open:

```text
\\Plugins\registry\HKCU\ControlPanel\Keybd
```

If `DeviceLockWhenSuspend` is missing, create it using `+Add value+`.

Set:

```text
Name:  DeviceLockWhenSuspend
Type:  DWORD
Value: 1
Base:  Decimal
```

Be careful with the spelling:

```text
DeviceLockWhenSuspend
```

Do not write `DevuceLockWhenSuspend`.

### Enable automatic device lock on backlight off

Open:

```text
\\Plugins\registry\HKCU\ControlPanel\BackLight
```

Find:

```text
AutoDeviceLockEnable
```

If it already exists and shows:

```text
Base:  Decimal
Value: 0
```

change it to:

```text
Base:  Decimal
Value: 1
```

If the value is missing, create it:

```text
Name:  AutoDeviceLockEnable
Type:  DWORD
Value: 1
Base:  Decimal
```

### Result

The final values should be:

```reg
[HKCU\ControlPanel\Keybd]
"DeviceLockWhenSuspend"=dword:00000001

[HKCU\ControlPanel\BackLight]
"AutoDeviceLockEnable"=dword:00000001
```

## 6. Optional local program archive checksums
The following files were also recorded in the local Windows Mobile setup archive.

These are local checksums, not official vendor checksums.

```text
67f8bc86196e6a29eada124cb444408eec3e3df84b6f615a5a765289ec88f78b  AlReader2.zip
75eae3934a3183670ac556b232972fda42ba67ed221fe71eea3b543803d35e0a  cecmd.arm.cab
ac5521d8c5c881d10b4a83072eaa30bd062d43d184966a8f74c8a86f5c99b03d  mini5wm.cab
e5d9cf316867ad498b7aabb65ef5552340a061b0a3b4eea655a0f34e77094e2c  tcmdpocketarm.cab
85dcc7ea4777ee996995f98bc299360605702ab73ce67484d15d4bdeb3e75043  tcmdpocketarm.exe
```

To recreate this file:

```sh
sha256sum AlReader2.zip cecmd.arm.cab mini5wm.cab tcmdpocketarm.cab tcmdpocketarm.exe > SHA256SUMS
```

To verify:

```sh
sha256sum -c SHA256SUMS
```
