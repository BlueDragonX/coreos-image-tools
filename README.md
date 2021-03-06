CoreOS Image Tools
==================
A set of tools for manipulating CoreOS images.

coreos-install
--------------
Install components into the root partition if a CoreOS image. Currently files
and systemd units are supported. The image is automatically mounted and
unmounted. Alternatively a directory may be given where an image is already
mounted.

For installing files:

    $ coreos-install file ROOT SRC DEST

  - ROOT is the image or directory to install to.
  - SRC may be a file or directory. It will be copied recursively.
  - Ownership of DEST will be recursively changed to root:root.

For installing units:

    $ coreos-install unit ROOT SRC

  - ROOT is the image or directory to install to.
  - SRC must be a file. It will be copied to /usr/lib64/systemd/system.
  - SRC will be added as a Want= to the top of the coreos-startup target.

coreos-fs
------------
Mount or unmount partitions in an image.

To mount a partition on an image:

    $ coreos-fs mount IMAGE LABEL MNT

  - The IMAGE is a GPT partitioned disk image.
  - The partition with LABEL will be mounted.
  - The mount point is MNT and must already exist.
  - Read-only root partitions are made writable.

To unmount a partition:

	$ coreos-fs umount IMAGE LABEL

  - Searches for the mounted image partition and unmounts it.
  - Detaches loop devices used during the process.
  - Does not remove the mount point.

Common labels are:

  - EFI-SYSTEM
  - ROOT-A
  - ROOT-B
  - OEM
  - STATE

See the CoreOS docs for additional partition details.

coreos-rw
---------
Enable or disable write mode on a CoreOS root partition.

To enable write mode:

    $ coreos-rw enable PARTITION

To disable write mode:

    $ coreos-rw disable PARTITION

To view the current byte value:

    $ coreos-rw get PARTITION | hexdump -b

Requirements
------------
The following system utilities need to be installed for these tools to work:

  - blkid
  - dd
  - kpartx
  - losetup
  - mount

License
-------
This software project is licensed under the BSD-derived license and is
copyright (c) 2014 Ryan Bourgeois. A copy of the license is included in the
LICENSE file. If it is missing then a copy may be found on the project page.
