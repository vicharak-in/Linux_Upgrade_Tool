# Linux Upgrade Tool

Rockchip's **Linux Upgrade tool** is a proprietary solution developed by the company
for flashing images onto various storage devices such as
`SPI`, `eMMC`, `SD Card`, and more.
Unlike open-source [rkdeveloptool](https://github.com/rockchip-linux/rkdeveloptool)
software, this tool does not provide access to its source code.

Instead, it is distributed solely in **binary executable** form,
allowing users to utilize the provided executable files for the purpose of
flashing images onto their desired storage devices.

## How to use Linux Upgrade Tool

### Installing required system dependencies

Linux Upgrade Tool requires the following dependencies to be installed
on your Debian or Ubuntu system.

```bash
sudo apt-get install libudev-dev libusb-1.0-0-dev
```

---

For other Linux distributions, please refer to the following table
for the equivalent package names.

|  Debian/Ubuntu   |    Fedora    | Arch Linux |
| :--------------: | :----------: | :--------: |
|   libudev-dev    |              |            |
| libusb-1.0-0-dev | libusb-devel |   libusb   |

---

### Linux Upgrade Tool Usage

```text
---------------------Tool Usage ---------------------
Help:             H
Version:          V
Log:              LG
------------------Upgrade Command ------------------
ChooseDevice:		CD
ListDevice:		LD
SwitchDevice:		SD
UpgradeFirmware:	UF <Firmware> [-noreset]
UpgradeLoader:		UL <Loader> [-noreset] [FLASH|EMMC|SPINOR|SPINAND]
DownloadImage:		DI <-p|-b|-k|-s|-r|-m|-u|-t|-re image>
DownloadBoot:		DB <Loader>
EraseFlash:		EF <Loader|firmware>
PartitionList:		PL
WriteSN:		SN <serial number>
ReadSN:			RSN
ReadComLog:		RCL <File>
CreateGPT:		GPT <Input Parameter> <Output Gpt>
SwitchStorage:		SSD
----------------Professional Command -----------------
TestDevice:		TD
ResetDevice:		RD [subcode]
ResetPipe:		RP [pipe]
ReadCapability:		RCB
ReadFlashID:		RID
ReadFlashInfo:		RFI
ReadChipInfo:		RCI
ReadSecureMode:		RSM
WriteSector:		WS  <BeginSec> <PageSizeK> <PageSpareB> <File>
ReadLBA:		RL  <BeginSec> <SectorLen> [File]
WriteLBA:		WL  <BeginSec> [SizeSec] <File>
EraseLBA:		EL  <BeginSec> <EraseCount>
EraseBlock:		EB <CS> <BeginBlock> <BlokcLen> [--Force]
RunSystem:		RUN <uboot_addr> <trust_addr> <boot_addr> <uboot> <trust> <boot>
-------------------------------------------------------
```

You can use the above command by using `sudo ./upgrade_tool` prefix
before any of the command.

**For example to erase flash:**

```bash
sudo ./upgrade_tool db <loader path>
sudo ./upgrade_tool ef
```

### Flashing RAW image using Linux_Upgrade_Tool

RAW images can be flashed to any storage device using the `dd` command or the `Linux_Upgrade_Tool`.

#### Check for connected devices

```bash
sudo ./upgrade_tool ld
```

```text
List of rockusb connected(1)

DevNo=1 Vid=0x2207,Pid=0x330c,LocationID=7143 Mode=Maskrom SerialNo=
```

#### Flash the loader binary

```bash
sudo ./upgrade_tool db rk3399_loader_xxx.bin
```

```text
Download boot ok.
```

#### Flash the RAW GPT image to storage device

> :warning:
> Make sure to flash the loader first to the storage device.

```bash
sudo ./upgrade_tool wl 0 <some_gpt_image>.img
```

Example:

```bash
sudo ./upgrade_tool wl 0 Vicharak_Vaaman_RAW_debian_bullseye_XFCE_beta_v0.1.0_08072023.img
```

```text
Write LBA OK.
```

#### Reboot the device

```bash
sudo ./upgrade_tool rd
```

```text
[sudo] password for vicharak:

Reset Device Success!
```

> :grey_exclamation:
> If you encounter **Reset Device Fail!** then try to manually reboot \
> using the power button on the board.

---

**Alternatively you can directly flash update image to eMMC
using `upgrade firmware` method.**

### Flash eMMC image using (upgrade firmware) method

```bash
sudo ./upgrade_tool uf <some_update_image>.img
```

Example:

```bash
sudo ./upgrade_tool uf Vicharak_Vaaman_EMMC_debian_bullseye_XFCE_beta_v0.1.0_03072023.img
```

```text
[sudo] password for vicharak:
Loading firmware...
Support Type:330C FW Ver:8.1.00 FW Time:2023-07-07 14:11:41
Loader ver:1.1e Loader Time:2023-07-07 14:11:08
Start to upgrade firmware...
Download Boot Start
Download Boot Success
Wait For Maskrom Start
Wait For Maskrom Success
Test Device Start
Test Device Success
Check Chip Start
Check Chip Success
Get FlashInfo Start
Get FlashInfo Success
Prepare IDB Start
Prepare IDB Success
Download IDB Start
Download IDB Success
Download Firmware Start
Download Image... (12%)
```
