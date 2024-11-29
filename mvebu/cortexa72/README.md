# OpenWrt Installation

## Common

1. Boot V-81 normally
1. Login to the vendor CLI (default: admin/admin) and login to the Linux CLI by `expert` command
1. Update U-Boot environment variables by the following commands

    ```
    fw_setenv bootcmd_ow_usb 'usb start; load usb 0:1 ${loadaddr} boot.scr && source ${loadaddr}'
    fw_setenv bootcmd_ow_sd 'load mmc 0:1 ${loadaddr} boot.scr && source ${loadaddr}'
    fw_setenv bootcmd_ow_emmc 'run set_mmc_internal; mmc read ${loadaddr} ${prim_header_mmc_blk} 4 && source ${loadaddr}'
    fw_setenv bootcmd 'run bootcmd_ow_usb; run bootcmd_ow_sd; run bootcmd_ow_emmc; run bootcmd_part${activePartition};'
    ```

    Attention: don't forget single quatations of values to prevent expansion of variables

1. Turn off the device

## USB/SD

Attention: Turn off the device before inserting MicroSD card (the stock firmware erases all contents in the first partition)

1. Burn (squashfs|ext4)-sdcard.img.gz to USB storage or MicroSD card

    example:
    - Linux: `gunzip -c <sdcard.img.gz> > /dev/sdX (or /dev/mmcblkN)`
    - Windows: use Rufus or something

1. Connect that storage to the USB 3.0 port or MicroSD slot on V-80 or V-81
1. Turn on V-80 or V-81 and it will be booted with OpenWrt in that storage

## eMMC

1. Copy initramfs image, dtb and bootsctipt to the USB storage with renaming

   - initramfs.bin -------> Image
   - dtb -----------------> V-80: armada-7040-v-80.dtb, V-81: armada-8040-v-81.dtb
   - bootscript (.scr) ---> boot.scr

2. Connect that storage to the USB 3.0 port on V-80 or V-81
3. Turn on V-80 or V-81 and it will be booted with OpenWrt initramfs image in that USB storage
4. Upload (squashfs|ext4)-sysupgrade.gz to V-80 or V-81
5. Perform sysupgrade with the uploaded image
6. Wait ~100 seconds to complete flashing

### Reverting to stock firmware

1. Turn on V-80 or V-81 and interrupt booting by Ctrl + C
2. Select "4. Restore to Factory Defaults (local)"
3. Wait ~180 seconds to complete restoring and rebooting
