# auto-UEFI-entry
An interactive tool to auto-generate UEFI-entries

aufii ist a fast way to create UEFI-entries.

aufii works like this:
aufii asks for the EFI partition, autodetects root and swap partitions with blkid, asks wether you need microcode and which kernel you use and AUTOMATICALLY CREATES THE NECESSARY COMMANDS FOR YOUR UEFI-ENTRIES with efibootmgr. You can check which devices will be used and wether the auto-composed commands are correct. aufii will create a small executable and if thou wilst - execute it.

aufii is interactive: while setting up your system you do not want to read man pages of commands you really seldomly need.

aufii is a very simple bash script inviting you to add some more functionality and contribute to a tool which is made to JUST SAVE TIME.

But carefully check the created commands, as wrong UEFI-entries will render a system unbootable. So you better have a live-system at hand when trying the tool late at night just for fun.

Simply run aufii. As it is very basic, you are invited to add more functions and commit.

Munich, March 2020, arl
