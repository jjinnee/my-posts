# Check if storage mounted

    ls /dev
    # /dev/sdb | /dev/sdc | ...

---

# Storage partition

    sudo fdisk -l
    sudo fdisk /dev/sdb
    type `p` to start
    enter ~
    type 'p` to check setup
    type `w` to save setup

After this works, check the partition that created just now

    ls /dev | find sdb1

---

# Storage format

    mkfs -t ext4 /dev/sdb1

---

# Mount storage

    sudo mkdir /data
    sudo chmod 777 /data
    sudo mount /dev/sdb1 /data
    ls /data

---

# Mount permanently

    sudo crontab -e
    # @reboot sudo mount /dev/sdb1 /data

---

# Unmount storage

    sudo umount [ Device_name || Mounted directory name ]
