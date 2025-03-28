# install virtual box
sudo apt-get install virtualbox
#  create a file
dd bs=512 if=/dev/zero of=hd.img count=204800
# check the loop device 
sudo losetup
#  set a file to device
sudo losetup /dev/loop0 hd.img
# format the file
sudo mkfs.ext4 -q /dev/loop0
# make a dir
mkdir hdisk
# mount the file
sudo mount -o loop ./hd.img ./hdisk/
# create the boot dictionary
sudo mkdir ./hdisk/boot/
# install the grup
sudo grub-install --boot-directory=./hdisk/boot/ --force --allow-floppy /dev/loop0
# create the config file:./hdisk/boot/grub/grub.cfg 
```
menuentry 'HelloOS' {
insmod part_msdos
insmod ext2
set root='hd0,msdos1' #我们的硬盘只有一个分区所以是'hd0,msdos1'
multiboot2 /boot/HelloOS.eki #加载boot目录下的HelloOS.eki文件
boot #引导启动
}
set timeout_style=menu
if [ "${timeout}" = 0 ]; then
  set timeout=10 #等待10秒钟自动启动
fi
```
# convert the disk format
VBoxManage convertfromraw ./hd.img --format VDI ./hd.vdi
VBoxManage convertfromraw ./hd.img --format VMDK ./hd.vmdk