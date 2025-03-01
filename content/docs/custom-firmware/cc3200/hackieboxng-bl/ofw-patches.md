---
title: "Firmware patches"
description: "Start and read!"
weight: 30
---

# HackieboxNG original firmware patches
## Preface
Every patch is written for one or more specific versions of the original firmware. Currently the latest version supported for all patches is **EU_V3.1.0_BF2-0**. An overview over all known firmware versions can be found in the [toniebox wiki](https://toniebox-reverse-engineering.github.io/docs/wiki/cc3200/firmware-list/).

## Offline / Privacy
It is recommended to keep the toniebox offline if you are using a patched firmware. Using the offline mode is recommended. In addition you should enable the Null certificate patch.

### Null certificate Paths ([noCerts.305.json](https://github.com/toniebox-reverse-engineering/hackiebox_cfw_ng/blob/master/sd-bootloader-ng/bootmanager/sd/revvox/boot/patch/noCerts.305.json))
This patch clears the paths to the certificates. This way the box will abort the https connection to boxine as it can't check if the boxine server is a valid one. 

## Alternative Tags (SLIX / SLIX2)
If you want to use alternative tags those patches will help you. Even other iso15693 tags may work.

### Block count >8 ([blockCheck.310.json](https://github.com/toniebox-reverse-engineering/hackiebox_cfw_ng/blob/master/sd-bootloader-ng/bootmanager/sd/revvox/boot/patch/blockCheck.310.json) / [blockCheck.307.json](https://github.com/toniebox-reverse-engineering/hackiebox_cfw_ng/blob/master/sd-bootloader-ng/bootmanager/sd/revvox/boot/patch/blockCheck.307.json))
Usally the toniebox checks if the tag has exactly 8 blocks. The check allows the tag to have more than that. (ex. SLIX or SLIX2)

### Block count <=8 ([blockCheckRemove.310.json](https://github.com/toniebox-reverse-engineering/hackiebox_cfw_ng/blob/master/sd-bootloader-ng/bootmanager/sd/revvox/boot/patch/blockCheckRemove.310.json) / [blockCheckRemove.308.json](https://github.com/toniebox-reverse-engineering/hackiebox_cfw_ng/blob/master/sd-bootloader-ng/bootmanager/sd/revvox/boot/patch/blockCheckRemove.308.json))
Usally the toniebox checks if the tag has exactly 8 blocks. The check allows the tag to have less than that.

### No privacy password ([noPass3.310.json](https://github.com/toniebox-reverse-engineering/hackiebox_cfw_ng/blob/master/sd-bootloader-ng/bootmanager/sd/revvox/boot/patch/noPass3.310.json) / [noPass3.305.json](https://github.com/toniebox-reverse-engineering/hackiebox_cfw_ng/blob/master/sd-bootloader-ng/bootmanager/sd/revvox/boot/patch/noPass3.305.json))
Usally doesn't allow tags without the boxine specific or the NXP specific privacy mode password. With the patch you can use tags without having privacy password support (ex. SLIX). It disables the rf field for a moment when the second privacy password failed. This patch uses the new dynamic patching engine to generate dynamic arm asm bytecode.

### Second privacy password to 00000000 ([secondPwZero.305.json](https://github.com/toniebox-reverse-engineering/hackiebox_cfw_ng/blob/master/sd-bootloader-ng/bootmanager/sd/revvox/boot/patch/secondPwZero.305.json))
Usally doesn't the toniebox checks for two passwords. The boxine specific and the NXP specific standard password *0f0f0f0f*. This patch changes the second password to *00000000*.


### No UID check ([uidCheck.307.json](https://github.com/toniebox-reverse-engineering/hackiebox_cfw_ng/blob/master/sd-bootloader-ng/bootmanager/sd/revvox/boot/patch/uidCheck.307.json))
Usally the toniebox checks if the UID of the tag starts with *E0:04:03*. With that patch you may use tags with other UIDs (ex SLIX or SLIX2)

### Hide A: Do not hide files that need new content ([noHide.308.json](https://github.com/toniebox-reverse-engineering/hackiebox_cfw_ng/blob/master/sd-bootloader-ng/bootmanager/sd/revvox/boot/patch/noHide.308.json))
*You just need one of the hide patches! This one is recommended*
Usally the toniebox sets the file attribute hidden of the tonie file for all live tags or tags having new content. If the toniebox is online and the tag is placed on top it will delete that file to redownload its content. This also applies to all custom tags. This patch disabled the hide function. So the files won't be deleted and redownloaded. **Attention** You won't be able to update creative tonies anymore!

### Hide B: Unhide files that should be hidden (because they have new content) ([alwaysUnhide.308.json](https://github.com/toniebox-reverse-engineering/hackiebox_cfw_ng/blob/master/sd-bootloader-ng/bootmanager/sd/revvox/boot/patch/alwaysUnhide.308.json))
*You just need one of the hide patches!*
Usally the toniebox sets the file attribute hidden of the tonie file for all live tags or tags having new content. If the toniebox is online and the tag is placed on top it will delete that file to redownload its content. This also applies to all custom tags. This patch replaces the hide functionality with unhide. So the files won't be deleted and redownloaded. **Attention** You won't be able to update creative tonies anymore!


## Additional
### Disable privacy mode ([noPrivacy.310.json](https://github.com/toniebox-reverse-engineering/hackiebox_cfw_ng/blob/master/sd-bootloader-ng/bootmanager/sd/revvox/boot/patch/noPrivacy.310.json) / [noPrivacy.305.json](https://github.com/toniebox-reverse-engineering/hackiebox_cfw_ng/blob/master/sd-bootloader-ng/bootmanager/sd/revvox/boot/patch/noPrivacy.305.json))
Usally the toniebox puts every tag into privacy mode after reading it. This patch disables that, so you can easily read the UID with any standard iso15693 reader like your phone.
### Load CA from c2.der ([altCa.305.json](https://github.com/toniebox-reverse-engineering/hackiebox_cfw_ng/blob/master/sd-bootloader-ng/bootmanager/sd/revvox/boot/patch/altCa.305.json))
To use two CAs you can use this patch. It will change the CA path from flash:/certs/ca.der to flash:/certs/c2.der
### Change Boxine URLs ([altUrl.305.json](https://github.com/toniebox-reverse-engineering/hackiebox_cfw_ng/blob/master/sd-bootloader-ng/bootmanager/sd/revvox/boot/patch/altUrl.305.json))
Changes the Boxine Cloud URLs from prod.de.tbs.toys to prod.revvox and rtnl.bxcl.de to rtnl.revvox.


##  Development
### Enable SWD ([swd.json](https://github.com/toniebox-reverse-engineering/hackiebox_cfw_ng/blob/master/sd-bootloader-ng/bootmanager/sd/revvox/boot/patch/swd.json))
Handy patch to debug the OFW and to verify and test patches.
***Attenion, the red led won't work with that patch enabled!***

## Deprecated Patches

### No privacy password ([noPass.305.json](https://github.com/toniebox-reverse-engineering/hackiebox_cfw_ng/blob/master/sd-bootloader-ng/bootmanager/sd/revvox/boot/patch/noPass.305.json))
*Deprecated doesn't reset the RF field so some tags will fail*
Usally doesn't allow tags without the boxine specific or the NXP specific privacy mode password. With the patch you can use tags without having privacy password support (ex. SLIX).

### No privacy password ([noPass2.308.json](https://github.com/toniebox-reverse-engineering/hackiebox_cfw_ng/blob/master/sd-bootloader-ng/bootmanager/sd/revvox/boot/patch/noPass2.308.json))
*Deprecated replaced with noPass3*
Usally doesn't allow tags without the boxine specific or the NXP specific privacy mode password. With the patch you can use tags without having privacy password support (ex. SLIX). It disables the rf field for a moment when the second privacy password failed.

### Disable charger wakeup ([noChargWake.305.json](https://github.com/toniebox-reverse-engineering/hackiebox_cfw_ng/blob/master/sd-bootloader-ng/bootmanager/sd/revvox/boot/patch/noChargWake.305.json))
The toniebox won't wakeup anymore if it is put onto the charger. **noChargWake.305 Doesn't work on 3.1.0-BF2 and higher. Box won't wakeup anymore** ***Attention, this patch is only working if you disconnect the battery for a second before loading the patched ofw. If you start the unpatched ofw once, you will have to disconnect the battery again***