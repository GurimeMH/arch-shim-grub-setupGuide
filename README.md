# arch-shim-grub-setupGuide
this is a repository that introduces how to setup secure boot on arch linux by shim with grub bootloader

## Follow the Arch Wiki
For detailed instructions, refer to:
- [Unified Extensible Firmware Interface (UEFI) Secure Boot](https://wiki.archlinux.org/title/Unified_Extensible_Firmware_Interface/Secure_Boot)
- [GRUB](https://wiki.archlinux.org/title/GRUB)

## GRUB Installation with Modules
To install GRUB with the required modules, use the following configuration:

```bash
GRUB_MODULES="ext2 fat part_gpt part_msdos linux search search_fs_uuid search_fs_file search_label normal efinet all_video boot btrfs cat chain configfile echo efifwsetup efinet ext2 fat font gettext gfxmenu gfxterm gfxterm_background gzio halt help hfsplus iso9660 jpeg keystatus loadenv loopback linux ls lsefi lsefimmap lsefisystab lssal memdisk minicmd normal ntfs part_apple part_msdos part_gpt password_pbkdf2 png probe reboot regexp search search_fs_uuid search_fs_file search_label serial sleep smbios squash4 test tpm true video xfs zfs zfscrypt zfsinfo cpuid play cryptodisk gcry_arcfour gcry_blowfish gcry_camellia gcry_cast5 gcry_crc gcry_des gcry_dsa gcry_idea gcry_md4 gcry_md5 gcry_rfc2268 gcry_rijndael gcry_rmd160 gcry_rsa gcry_seed gcry_serpent gcry_sha1 gcry_sha256 gcry_sha512 gcry_tiger gcry_twofish gcry_whirlpool luks lvm mdraid09 mdraid1x raid5rec raid6rec http tftp"

sudo grub-install --target=x86_64-efi --efi-directory=/boot/efi/ --modules="$GRUB_MODULES" --sbat /usr/share/grub/sbat.csv
```

## Mistake in EFI Boot Entry Creation
When setting up the EFI boot entry, I made the mistake of specifying the wrong loader directory. The incorrect command:
```bash
efibootmgr --unicode --disk /dev/sda --part 1 --create --label "Arch-shim" --loader /boot/efi/EFI/BOOT/BOOTX64.EFI
```
The correct command should have been:
```bash
efibootmgr --unicode --disk /dev/sda --part 1 --create --label "Arch-shim" --loader /EFI/BOOT/BOOTX64.EFI
```

## Initial Boot Setup
Before reinstalling GRUB, my EFI entries were:
- **Arch-grub-test**: `\EFI\Arch-grub-test\grubx64.efi` (currently in use)
- **USB Drive Boot**: `\EFI\BOOT\BOOTX64.EFI` (for specific situations when booting from USB)

## After Reinstalling GRUB with Modules
Post-reinstallation, my EFI entries were:
- **Arch-grub-test**: `\EFI\Arch-grub-test\grubx64.efi` (currently in use)
- **USB Drive Boot**: `\EFI\BOOT\BOOTX64.EFI` (for specific situations when booting from USB)
- **Installed with Modules**: `\EFI\arch\grubx64.efi`

To ensure bootability, I had to copy `\EFI\arch\grubx64.efi` to `\EFI\BOOT\BOOTX64.EFI`.

## Useful Commands
### Find the installed GRUB binary:
```bash
find /boot -name "grubx64.efi"
```
### Create an EFI boot entry manually:
```bash
sudo efibootmgr --unicode --disk /dev/sdX --part Y --create --label "grubName" --loader /EFI/BOOT/BOOTX64.EFI
```
### Generate a new GRUB configuration:
```bash
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
# OR
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

