# so-root = squashed and overlayed folders in /

mounting squashed images as read-only loop devices and making them writeable with the help of overlayfs during early userspace
providing incremental and whole image creation options in userspace (inspired by squashfu)

# this is a work in progress

this project was inspired by ramroot while waiting for the thousands of files to get copied during boot from a random flash drive

# what works:
- folders like /usr /var /etc /home were tested successfully
- config file in /etc/so-root.conf
- $1 can be "/path/to/custom/*.conf" - sourcing that instead of the default config file
- $1 can be "/path/to/something.qcow2"
   - mount - mounting my 2 partitions from the qcow2 image at /mnt
   - update - pushing updates to the image
   - umount - unmounting /mnt

# how to try it:
- create manually the squashed images-0-0.sfs under your DIR_IMAGES
- make a test-run:
  - set DIR_DESTINATION of something else than "/"
  - include so-root in your HOOKS
  - run 'mkinitcpio -P'
  - reboot
  - verify with 'mount', lower,upper,workdirs include the tag /new_root in their path, but they do function properly in userspace
- do it for real
  - edit the DIR_DESTIONATION to "/"
  - run 'mkinitcpio -P'
  - reboot

# todo:
- rewrite the early_userspace functions for /bin/ash compatibility
- add/remove "so-root" to/from HOOKS before fsck with sed during package installation/removal
- create systemd unit for shutdown routines (umount_overlays(create_new_image), umount_images)
- come up with revert solution (haven't tried yet)
- create AUR package
