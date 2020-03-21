# auto-UEFI-entry
An interactive tool to auto-generate UEFI-entries

aufii ist the fastest way to create UEFI-entries.

aufii works like this:
aufii autodetects boot, root and swap partitions with blkid, asks you wether you need intel-ucode and which kernel you use and AUTOMATICALLY CREATES THE NECESSARY COMMANDS FOR YOUR UEFI-ENTRIES with efibootmgr. You can check which devices will be used and wether the auto-composed commands are correct. aufii will create a small executable and if thou wilst - execute it.

aufii is interactive: while setting up your system you do not want to read man pages of commands you really seldomly need.

aufii is designed for nvme disks but can be easily used with other disks too, just edit the executable to your fit your system.

aufii is a very simple bash script inviting you to add some more functionality and contribute to a tool which is made to JUST SAVE TIME.

But carefully check the created commands, as wrong UEFI-entries will render a system unbootable. So you better have a live-system at hand when trying the tool late at night just for fun.

Simply run aufii.
