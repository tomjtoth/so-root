# so-root = squashed and overlayed folders in /

mounting squashed images read-only as loop devices and making them writeable with the help of overlayfs, providing incremental and whole image creation options (inspired by squashfu)

# this is a work in progress

inspired by ramroot while waiting for the thousands of files (from ncdu: Items: 119038) to get copied during boot (using my personal setup) from a random Kingston flash drive (high seq. read, ~zero random read)

# what works:
 - qemu image mounts /usr /var /etc during early userspace and successfully switches to /new_root
 - config file in /etc/so-root.conf
 - $1 can be "/path/to/custom/*.conf" - sourcing that instead of the default config file
 - $1 can be "/path/to/something.qcow2" - 
    - mount - mounting my 2 partitions from the qcow2 image at /mnt
    - copy - pushing updates to the image
    - umount - unmounting /mnt
 
# worked, might be broken currently:
 - creation of incremental and seed images require a systemd service to kick in during poweroff and be able to quistionnaire the user

# how to try it:
 - create manually the squashed images-0-0.sfs under your DIR_IMAGES
 - make a test-run:
    - set DIR_DESTINATION of something else than "/"
    - include so-root in your HOOKS (I use the last place it's a late_hook anyways)
    - run 'mkinitcpio -P'
    - reboot
    - verify with 'mount', lower,upper,workdirs include '/new_root' in their path, but they do function properly
 - do it for real
    - edit the DIR_DESTIONATION to "/"
    - run 'mkinitcpio -P'
    - poweroff
    - mount your "/" partition on a different host and delete /whatever_folders_you_want_to_substitute 
    - poweron

# todo:
 - re-write in /bin/ash (I currently have 0 XP in it, so I bring /bin/bash to initarmfs as well)
 - split the script into so-root-load and so-root-unload parts, so unloading wouldn't have to be brought to initramfs
 - create systemd unit for shutdown routines (umount_overlays(create_new_image), umount_images)
 - automate image creation (currently doing it manually, offline)
 - come up with revert solution (haven't tried yet)
 - create AUR package
