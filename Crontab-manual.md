# Installation

    sudo apt install cron
    crontab -e

# Reboot at 5:00 everyday

> Must be entered in Server Time.
>
> Check `date` command.

    0 20 * * * sudo reboot

# Check update at 4:00 everyday

    0 19 * * * sudo apt update -y && sudo apt upgrade -y

# Interval

    * * * * * ~ : 1 min
    */5 * * * * : 5 mins

# Run shell script

    * * * * * bash [Filename].sh

# Restoring iptables settings

    @reboot sudo iptables-restore < [backup_file]

# Apply settings

    sudo service cron restart (reload)
