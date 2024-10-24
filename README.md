# Increase-ubuntu-disk-partation


To expand a disk partition in Ubuntu using `parted`, follow these steps carefully. This assumes you're resizing a partition on `/dev/sdb` and that you have a clear understanding of the changes you're about to make.

### Important:
- **Backup your data** before resizing partitions. Mistakes can lead to data loss.
- Ensure the partition you're resizing is **unmounted** before you begin.
- You need to have `parted` installed.

### Steps to Resize a Partition using `parted`

1. **Check Current Partition Table**
   Open a terminal and type:
   ```bash
   sudo parted /dev/sdb print
   ```
   This shows the current partition layout of `/dev/sdb`. Identify which partition (e.g., `/dev/sdb1`, `/dev/sdb2`) you want to resize.

2. **Unmount the Partition (if necessary)**
   If the partition is mounted, unmount it before making changes:
   ```bash
   sudo umount /dev/sdb1
   ```
   Replace `/dev/sdb1` with the correct partition name if necessary.

3. **Enter `parted` interactive mode**
   Enter the interactive mode for `/dev/sdb`:
   ```bash
   sudo parted /dev/sdb
   ```

4. **Resize the Partition**
   1. List all partitions:
      ```bash
      print
      ```
   2. Select the partition to resize:
      ```bash
      resizepart <partition_number> <end_size>
      ```
      Example:
      ```bash
      resizepart 1 100%
      ```
      - Here `1` refers to partition 1, and `100%` means to use the remaining space on the disk.
      - You can specify the size in GB, e.g., `resizepart 1 50GB`.

5. **Quit `parted`**
   After resizing:
   ```bash
   quit
   ```

6. **Resize the Filesystem**
   After adjusting the partition size, you need to resize the filesystem to match the new partition size.

   For **ext4** filesystems:
   ```bash
   sudo resize2fs /dev/sdb1
   ```
   Replace `/dev/sdb1` with your partition.

   For **xfs** filesystems:
   ```bash
   sudo xfs_growfs /dev/sdb1
   ```

7. **Mount the Partition Again**
   Mount the partition back to its location if necessary:
   ```bash
   sudo mount /dev/sdb1 /your/mount/point
   ```

### Verify the changes
To verify that the partition and filesystem have been resized, use:
```bash
df -h
```
This will show the new available space on the resized partition.

Let me know if you encounter any issues!
