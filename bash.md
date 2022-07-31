### Add cron jobs

    cat << EOF | (sudo) crontab
    $(sudo crontab -l)
    The text you want to add
    EOF

### Delete cron jobs

    (sudo) crontab -l | grep -v "The text you want to delete" | (sudo) crontab -
    
### No interactive

    sudo DEBIAN_FRONTEND=noninteractive apt dist-upgrade -y
