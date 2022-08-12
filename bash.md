### Cron jobs

> ### Add cron jobs

    cat << EOF | (sudo) crontab
    $(sudo crontab -l)
    The text you want to add
    EOF

> ### Delete cron jobs

    (sudo) crontab -l | grep -v "The text you want to delete" | (sudo) crontab -
    
### No interactive

    sudo DEBIAN_FRONTEND=noninteractive apt dist-upgrade -y

### Print line number you want

> ### head
> It is used to print the first n number of lines.

    head -n [number] [filename]
    cat [filename] | head -n [number]
    
> ### tail
> It is used to print the last n number of lines.

    tail -n [number] [filename]
    cat [filename] | tail -n [number]
    
> ### one line

    cat [filename] | sed -n '10p' # 10 line
    cat [filename] | awk "NR == 10" # 10 line
    
> ### middle of text

    cat [filename] | awk "NR == [number]"
    ----------------------------------------
    start=5
    end=10
    cat [filename] | awk "NR >= $((start + 1)) && NR <= $((end - 1))"
    ----------------------------------------
    cat [filename] | sed -n '10,20p' # 10 ~ 20 lines

### split text with one word

> ### cut

    text="one_two_three_four"
    echo "$text" | cut -d '_' -f 2 # Result is "two"
    
    
    
