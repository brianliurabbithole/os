# os

# move bin file
```
move helloOS.bin /boot/
```

# modify the cfg
```
/boot/grub/grub.cfg

add config to the end of the file:
menuentry 'HelloOS' {
     insmod part_msdos #GRUB加载分区模块识别分区
     insmod ext2 #GRUB加载ext文件系统模块识别ext文件系统
     set root='hd0,msdos4' #注意boot目录挂载的分区，这是我机器上的情况
     multiboot2 /boot/HelloOS.bin #GRUB以multiboot2协议加载HelloOS.bin
     boot #GRUB启动HelloOS.bin
}
```

# check the file system
```
sudo fdisk -l /dev/sda
Disk /dev/sda: 20 GiB, 21474836480 bytes, 41943040 sectors
Disk model: VMware Virtual S
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 0614B012-A221-4B8E-8FD7-5FF94CD6DF47

Device     Start      End  Sectors Size Type
/dev/sda1   2048     4095     2048   1M BIOS boot
/dev/sda2   4096 41940991 41936896  20G Linux filesystem

if Diskable is gpt, modify the insmod to part_gpt, change the `set root='hd0,msdos4'` to `set root='hd0,gpt4'`
```


# check the section of the start section
```
df /boot/ to 
文件系统          1K-块    已用     可用      已用% 挂载点
/dev/sda4      48752308 8087584 38158536   18%    /

change the `set root='hd0,msdos4'` msdos{numner} according to the filenumber
```
