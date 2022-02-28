# Enable swap

    fallocate -l 2GB /swapfile

    # Change permission
    chmod 600 /swapfile

    # Make swap
    mkswap /swapfile

    # Turn on swap
    swapon /swapfile

    # Turn off swap
    swapoff /swapfile || -a
    sudo rm /swapfile

## Crontab

    sudo crontab -e
    > @reboot sudo swapon /swapfile

    sudo service cron reload

---

# Swappiness

The kernel parameter that defines how much (and how often) your Linux kernel will copy RAM contents to swap.

<kbd>Default value</kbd> : 60 (0 ~ 100)

The higher the value of the swappiness parameter, the more aggressively your kernel will swap.

## Setup

- 0 : swap is disable
- 1 : minimum amount of swapping without disabling it entirely
- 10 : recommended value to improve performance when sufficient memory exists in a system
- 100 : aggressive swapping

### Immediate application

    sysctl -w vm.swappiness = <number>

### Permanent application

    sudo nano /etc/sysctl.conf
    > ...
    > vm.swappiness = 0 ~ 100

    sudo reboot
