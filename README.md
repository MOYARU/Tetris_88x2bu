<div align="center">
  
# RTL88x2BU for Nothing CMF Phone 1
**RTL8812BU / RTL8822BU USB Wi-Fi driver** for the Nothing **CMF Phone 1**
(`tetris`, MediaTek MT6878) on **Kali NetHunter LITE** — with
**monitor mode + frame injection**
`aireplay-ng --test` result : **30 / 30 (100%)**
The CMF is a really cool phone! I really wanted to run Kali Linux NetHunter, monitoring, and injection on this device.

</div>

## Kernel Compatibilit
This file works only with the corresponding kernel.
```
6.1.145-android14-11-g09f1c0074ad7-ab14226177
```
Check yours:
```bash
adb shell su -c 'uname -r'
```

| Your `uname -r` | Result |
|---|---|
| **Exactly** the string above | The prebuilt `.ko` should load |
| Anything different | `insmod: Invalid argument` — you must rebuild |
## works
| Feature | Status |
|---|---|
| Driver loads (`insmod`) | ✅ |
| `wlanX` interface appears | ✅ |
| Managed mode (scan / connect) | ✅ |
| **Monitor mode** | ✅ |
| **Frame injection** (`aireplay-ng --test`) | ✅ **30/30** |

**Tested adapter:** TP-Link **Archer T4U v3** (`2357:0115`, RTL8822BU).
Other RTL8812BU / RTL8822BU adapters in the driver's USB ID table should
work too.

## Quick start (matching vermagic)
```bash
# grab 88x2bu.ko from Releases, then:
adb push 88x2bu.ko /data/local/tmp/
adb shell su -c 'insmod /data/local/tmp/88x2bu.ko'
adb shell su -c 'ip link | grep wlan'      # wlanX should appear
```
Monitor mode + injection (inside the NetHunter chroot):
```bash
ip link set wlan1 down
iw dev wlan1 set type monitor
ip link set wlan1 up
iw dev wlan1 set channel 6
aireplay-ng --test wlan1
```
## Credits
- [**morrownr/88x2bu-20210702**](https://github.com/morrownr/88x2bu-20210702) — the driver. All credit upstream.
- **Realtek** — original source.
- **NothingOSS** — public MT6878 kernel source.
