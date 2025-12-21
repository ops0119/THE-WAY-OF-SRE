
**描述：** SystemRescue（以前称为 SystemRescueCd）是一个 Linux 系统救援工具包，可作为可启动介质使用，用于在崩溃后管理或修复系统和数据。它旨在提供一种在计算机上执行管理任务的简便方法，例如创建和编辑硬盘分区。它附带许多[Linux 系统实用程序，](https://www.system-rescue.org/System-tools/) 例如 GParted、fsarchiver、文件系统工具和基本工具（编辑器、午夜指挥官、网络工具）。它可以用于 Linux 和[Windows](https://www.system-rescue.org/manual/Backup_data_from_an_unbootable_windows_computer/) 计算机，也可以用于台式机和服务器。此救援系统不需要安装，因为它可以从 CD/DVD 驱动器或 [USB 记忆棒](https://www.system-rescue.org/Installing-SystemRescue-on-a-USB-memory-stick/)启动，但如果您愿意，也可以将其 [安装在硬盘上](https://www.system-rescue.org/manual/Installing_SystemRescue_on_the_disk/) 。内核支持所有重要的文件系统（ext4、xfs、btrfs、vfat、ntfs）以及网络文件系统，例如 Samba 和 NFS。

[https://www.system-rescue.org/Download/](https://www.system-rescue.org/Download/)

[https://fastly-cdn.system-rescue.org/releases/12.00/systemrescue-11.03-amd64.iso](https://fastly-cdn.system-rescue.org/releases/12.00/systemrescue-11.03-amd64.iso)
~~~bash
mount /dev/vda1 /mnt
cd / && for i in sys dev proc; do mount --bind /$i /mnt/$i;done
chroot /mnt
~~~

