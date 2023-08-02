# linux-on-matebook-14-2022
Running Linux on huawei matebook 14 2022 intel(KLVF-XX)

![图片](https://github.com/alim0x/linux-on-matebook-14-2022/assets/4954007/bed9763e-1688-4be9-963f-34e9a78f00fa)

| Device | Model |  Works | Notes |
| --- | --- |  :---: | --- |
| Processor | Intel Core i5-1240P | ✔ Yes | 12 cores, 4 Performance-cores + 8 Efficient-cores |
| Graphics | Intel® Iris® Xe Graphics | ✔ Yes |  |
| Memory | 16GB LPDDR4x 3733Mhz | ✔ Yes |  |
| Display | 14 inch 2:3, 2160x1440 (2K) 185 PPI | ✔ Yes |  |
| Storage | 512 GB NVMe PCIe SSD | ⚠️ Yes | problem with wakeup from sleep, see [Sleep](#sleep) |
| Wifi | Intel AX201 (a/b/g/n/ac/ax) | ✔ Yes |  |
| Bluetooth | Intel AX201 Bluetooth | ✔ Yes |  |
| Soundcard  | Intel Corporation Alder Lake PCH-P High Definition Audio Controller | ⚠️ Yes  | see [Soundcard](#soundcard) for details |
| Speakers  |  | ⚠️ Yes | see [Soundcard](#soundcard) for details |
| Microphone | | ⚠️ Yes | see [Soundcard](#soundcard) for details |
| Webcam | HD 720P | ✔ Yes |  |
| Ports | USB-C × 1<br>USB3.2 Gen 1 × 2<br>HDMI × 1<br>3.5 mm headset and microphone 2-in-1 jack × 1 | ✔ Yes | USB-C support data, charging and DisplayPort |
| Power button |  | ✔ Yes |  |
| Fingerprint Reader | Goodix Unknown (27c6:51c0) | ❌ No | located on the power button, see [Fingerprint](#fingerprint) |
| Battery | 56 Wh | ✔ Yes |  |
| Lid |  |  ✔ Yes |  |
| Power management | |  |  |
| Keyboard |  | ✔ Yes |  |
| Touchpad |  | ✔ Yes |  |

### Sleep

Can't wakeup from sleep. All hardware is waking up except SSD drive, maybe it is because that the nvme has entered power saving state.

```
Supported Power States
St Op     Max   Active     Idle   RL RT WL WT  Ent_Lat  Ex_Lat
 0 +     9.00W       -        -    0  0  0  0        0       0
 1 +     4.60W       -        -    1  1  1  1        0       0
 2 +     3.80W       -        -    2  2  2  2        0       0
 3 -   0.0450W       -        -    3  3  3  3     2000    2000
 4 -   0.0040W       -        -    4  4  4  4    15000   15000
```

As a workaround, add the kernel parameter `nvme_core.default_ps_max_latency_us=0` to completely disable APST, or set a custom threshold to disable specific states. For example, to disable PS4 set `nvme_core.default_ps_max_latency_us=2000`. 

See:

* [\[Archwiki\] NVMe#Controller_failure_due_to_broken_APST_support](https://wiki.archlinux.org/title/Solid_state_drive/NVMe#Controller_failure_due_to_broken_APST_support)

And the other way you can do with sleep problem:

**BUY A NEW SSD DRIVE AND REPLACES OLD ONE. IT WORKS.**

### Soundcard

Works with most recent sof-firmware(v2.2.5 or newer), but not perfect.

Sound volume too low, need to adjust volume using `alsamixer`, set DAC to 100%.

Microphone not working by default, use `alsa-ucm-conf` and select `pro audio` profile can see microphone device, but may cause other sound problems.

See Github issue: [\[ADL-P\] Missing topology for ES8336 Alder Lake P device (i5-1240P Huawei Matebook 14 2022)](https://github.com/thesofproject/linux/issues/4111)

### Fingerprint

Goodix Unknown (27c6:51c0), even nothing about fingerprint reader shows in `lsusb`.

```
Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 003 Device 002: ID 13d3:5476 IMC Networks HD Camera
Bus 003 Device 003: ID 8087:0026 Intel Corp. AX201 Bluetooth
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
```
