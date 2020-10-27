# so-root = squashed and overlayed folders in /

mounting squashed images as read-only loop devices and making them writeable with the help of overlayfs during early userspace
providing incremental and whole image creation options in userspace (inspired by squashfu)

# this is a work in progress

this project was inspired by ramroot while waiting for the thousands of files to get copied during boot from a random flash drive

# what works:
- folders like /usr /var /etc /home were tested successfully
- config file in /etc/so-root.conf
- saving the state of the upper folders on user request
- some test functions

# how to try it:
- copy the files in place
- run 'so-root --enable'
- include "keymap" and "so-root" in your HOOKS
- make a test-run:
  - set DIR_DESTINATION of something else than "/"
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
