#!/bin/bash
### Automatically generate UEFI-boot entries
usage() { printf "Usage:\t aufii \naufii is a simple interactive tool to automatically generate UEFI boot entries. It generates efibootmgr commands and exports them to a small executable. You can safely run the script as it does write nothing without further confirmation. \n [-h]\t<display help>\n "; 1>&2; exit 1; }


# Compose [and execute] efibootmgr commands 
while getopts :h OPT; do
	case ${OPT} in
		h)
		usage;;
	esac
done
shift $((OPTIND-1))
read -r -p "Simple interactive tool to create UEFI-boot entries. No changes will be written to disk before confirmation. Start now (y/n)? " CHOICE
case ${CHOICE} in
y)	
echo ...
echo ...
read -r -p "Please specify EFI partition: " str

# Detect partitions
#EFI=$(blkid | grep EFI | awk -F ':' '{print $1}')

EFI=${str}
EFIU=$(blkid | grep "${EFI}" | awk -F '"' '{print $2}')
DISK=$(echo "${EFI}" | awk -F 'p' '{print $1}')
PART=$(echo "${EFI}" | awk -F 'p' '{print $2}')
AROT=$(lsblk| grep -w "/"|awk -F ' ' '{print $1}'|tail -c4)
ROOT=$(blkid | grep "${AROT}"| awk -F '"' '{print $2}')
SWAP=$(blkid | grep swap | awk -F '"' '{print $2}')
P_S=$(blkid | grep "${SWAP}" | awk -F ':' '{print $1}')
P_E=$(blkid | grep "${EFI}" | awk -F ':' '{print $1}')
P_R=$(blkid | grep "${ROOT}"| awk -F ':' '{print $1}')

echo ...
echo ...
read -r -p "Include microcode (amd/intel/no)? (a/i/n) " CHOICE
case ${CHOICE} in
a)
echo "Including amd-ucode"
UCODE="initrd=\amd-ucode.img"
echo ...
echo ...;;
i)
echo "Including intel-ucode"
UCODE="initrd=\intel-ucode.img"
echo ...
echo ...;;
n)
echo "Not using microcode"
echo ...
echo ...;;
esac;;
n)
echo "Aborted by user"
exit 1;;
esac
shift $((OPTIND-1))

# Choose kernel
read -r -p "Choose your kernel: linux, linux-hardened, linux-lts, linux-zen (l/h/s/z) " CHOICE
case ${CHOICE} in
l) 
echo "kernel is linux"
echo ...
echo ...
KERN="";;
h) 
echo "kernel is linux-hardened"
echo ...
echo ...
KERN="-hardened";;
p) 
echo "kernel is linux-lts"
echo ...
echo ...
KERN="-lts";;
z) 
echo "kernel is linux-zen"
echo ...
echo ...
KERN="-zen";;
esac
shift $((OPTIND-1))

# Name
read -r -p "Please label the boot entry (e.g. Arch-Linux): " str
NAME=${str}
shift $((OPTIND-1))

# Compose commands
FLBK=$(echo "efibootmgr --disk ${DISK} --part ${PART} --create --label \"${NAME}-Fallback\" --loader /vmlinuz-linux${KERN} --unicode 'root=UUID=${ROOT} resume=UUID=${SWAP} rw ${UCODE} initrd=\initramfs-linux${KERN}-fallback.img'")
LINX=$(echo "efibootmgr --disk ${DISK} --part ${PART} --create --label \"${NAME}\" --loader /vmlinuz-linux${KERN} --unicode 'root=UUID=${ROOT} resume=UUID=${SWAP} rw ${UCODE} initrd=\initramfs-linux${KERN}.img'")

# Prompt detected partitions and composed commands
printf "Partitions detected:\nEFI:\t${P_E}\t${EFIU}\nRoot:\t${P_R}\t${ROOT}\nSwap:\t${P_S}\t${SWAP}\n"
echo ...
echo ...
echo "Composed commands:"
echo ...
echo "${FLBK}"
echo ...
echo "${LINX}"
echo ...
echo "To add additional kernel parameters just choose the first option in the next step and edit the file before executing it."
echo ...
echo ...
# Write to disk and execute or abort
write_exec (){
echo "#!/bin/bash" > UEFI_gen${KERN}
echo "${FLBK}" >> UEFI_gen${KERN}
echo "${LINX}" >> UEFI_gen${KERN}
echo "exit 0" >> UEFI_gen${KERN}
echo "# See man efibootmgr" >> UEFI_gen${KERN}
chmod +x UEFI_gen${KERN}
printf "Commands written to file UEFI_gen${KERN}"
}

read -r -p "Create executable, create and execute (sets UEFI boot entries) or abort (c/ce/a)? " CHOICE 
case ${CHOICE} in
c)  # write to file
write_exec;;
ce) # write to file and execute it
write_exec
./UEFI_gen${KERN} 
printf "Changes written, poweroff and restart, don't reboot.";;
a)  # abort
printf "Aborted, no changes written to disk.\n";;
esac
shift $((OPTIND-1))
exit 0
