# so-root
squashed and overlayed folders in /

# this is a work in progress

inspired by ramroot while waiting for the thousands of files (from ncdu: Items: 119038) to get copied during boot (using my personal setup).

# what works:
 - qemu image mounts /usr /var /etc during early userspace and successfully switches to /new_root
 
# untested in with /folders, but should work once systemd service is done:
 - creation of incremental and seed images
 
# todo:
 - re-write in /bin/ash (I currently have 0 XP in it, so I added /bin/bash to the image as well)
 - split the script into so-root-load and so-root-unload parts
 - edit /etc/mkinitcpio.conf
 - create systemd unit for shutdown routines (umount_overlays(create_new_image), umount_images)
 - automate image creation (currently doing it manually)
 - come up with revert solution (haven't tried)
 - come up with an install solution (currently doing manually offline on the boot medium)
 - create AUR package
