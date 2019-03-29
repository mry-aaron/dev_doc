# VirtualBox更改虚拟硬盘VDI文件空间大小

## 启动前设置

> * 启动CMD命令行，进入VirtualBox安装目录
> * 执行VBoxManager.exe实现空间大小设置
>   * VBoxManager.exe modifyhd xxx.vdi --resize SIZE_IN_MB
>     * xxx.vdi ：指vdi文件的全路径
>     * SIZE_IN_MB ：指修改后的硬盘容量，单位MB
>   * Eg: VBoxManage.exe modifyhd "E:\dds\VirtualBox VMs\linux\linux-bak.vdi" --resize 15360
>   * 执行结果：0%....10%....20%....30%....40%....50%....60%....70%....80%....90%....100%

## 启动后设置

### 查看新的磁盘空间

> \# fdisk -l /dev/sda

### 启用新增加的空间

> * 使用 fdisk 将虚拟磁盘的空闲空间创建为一个新的分区
>
>   ```xml
>   # fdisk /dev/sda     ---用root用户操作(执行命令后根据提示依次输入)
>   	n {new partition}   
>   	p {primary partition}
>   	3 {partition number}
>   	（提示修改大小，选择默认直接回车即可）
>   	t {change partition id}   ---这个的目的是将ID 修改成8e的类型，即LVM
>   	3 {partition number}
>   	8e {Linux LVM partition}
>   	w
>   ```
>
> * 重启系统使分区生效（reboot）
>
> * 查看新增分区是否标记为LVM
>
>   \# fdisk -l
>
>   \# fdisk -l /dev/sda3
>
> * 调整LVM大小
>
>   \# vgdisplay （查看VolumeGroup名称，实际操作时用）
>
> * 在新空间上创建新的物理卷
>
>   \# pvcreate /dev/sda3
>
>   \# vgextend VG名称 /dev/sda3 （使用新的物理卷扩展LVM的VolGroup）
>
>   \# df -h （查看空间使用情况）
>
>   \# lvextend /dev/mapper/VG名称 /dev/sda3 （扩展LVM逻辑卷）
>
>   \# resize2fs /dev/mapper/VG名称 （调整逻辑卷大小）
>
>   ​	-- 注意：由于不同版本的文件系统类型不一样，CentOS7使用的是xfs类型，使用命令xfs_growfs
>
>   ​	-- Eg：xfs_growfs /dev/mapper/VG名称
>
>   \# df -h （查看效果）
>
>   















































