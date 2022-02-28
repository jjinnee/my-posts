# echo color

![123123213](https://user-images.githubusercontent.com/46839654/111976624-bd28f580-8af9-11eb-8631-2bf57ff2976f.png)

    echo "TEXT COLOR"
    echo -e "\e[1;31m RED \e[0m"
    echo -e "\e[1;32m GREEN \e[0m"
    echo -e "\e[1;33m YELLOW \e[0m"
    echo -e "\e[1;34m BLUE \e[0m"
    echo -e "\e[1;35m MAGENTA \e[0m"
    echo -e "\e[1;36m CYAN \e[0m"
    echo ""
    echo "BACKGROUND COLOR"
    echo -e "\e[1;41m RED \e[0m"
    echo -e "\e[1;42m GREEN \e[0m"
    echo -e "\e[1;43m YELLOW \e[0m"
    echo -e "\e[1;44m BLUE \e[0m"
    echo -e "\e[1;45m MAGENTA \e[0m"
    echo -e "\e[1;46m CYAN \e[0m"

    # Usage
    BG_GREEN="\e[1;42m"
    NC="\e[0m"

    echo -e "${BG_WHITE}1234${NC}"

---

# Variable

- Create

        str="123"
        var=1
        arr=("string" "string" "string")
        arr1=(a b c)

- Call

        echo "$str"
        echo ${var}
        echo ${arr[2]}

- Remove

        unset str
        unset var
        unset arr[1]

---

# Run command with variable

    str="String"
    date="03-22-21"
    arr=(1 2 3)

    echo "${str}"
    > String

    echo "abcd${str}"
    > abcdString

    echo ${arr[1]}
    > 2

    echo ${arr[@]}
    > 1 2 3

    echo "abcd${arr[1]}"
    > abcd2

    grep ${date} | <file>

    sudo apt install ${var} -y

---

# Receive inputs from user

- read

        #!/bin/bash
        printf "input : "
        read ip

        echo "$ip"
        or
        sudo fail2ban-client set <jail_name> unbanip ${ip}

- read -p

        #!/bin/bash
        echo "input"
        read -p "ip : " ip

        echo "$ip"

- Input default

        #!/bin/bash
        read -p "Do you want to install? (y/n) [n] : " agree

        // Check if variable is blank variable
        [ -z "$agree" ] && agree = "n"

        // Verify variable is not blank
        // [ -n "$agree" ] && ~

        echo "$agree"

        // When entering whitespace
        > n
        // When entering "y"
        > y

---

# Array

    #!/bin/bash
    arr=""

    for(( i=1; i<5; i++ )); do
        arr[${i}]=${i}
    done

    for(( i=1; i<5; i++ )); do
        echo "${arr[$i]}" or ${arr[$i]}
    done

    > 1
    > 2
    > 3
    > 4

---

# Repetition

- while

        while <condition>; do ~ done

        #!/bin/bash
        i=1

        while [[ "$i" < 4 ]]; do
                echo "$i"
                ((i=i+1)) // i = i + 1 == i++
        done
        > 1
        > 2
        > 3

- for

        for <variable> in 1 2 3 4 5; do ~ done

        #!/bin/bash
        arr=(str1 str2 str3)

        for item in ${arr[@]}; do // arr[@] means "str1 str2 str3"
            echo "$item"
        done
        > str1
        > str2
        > str3

---

# IF and CASE

    #!/bin/bash
    echo "1) view banned ip"
    echo "2) unbanip"
    read -p "type [1] : " type

    if [ $type = 1 ]; then
        sudo fail2ban-client status <jail_name>
        read -p "Do you want unbanip? (y/n) : " agree

        case $agree in
            y) // if $agree is y
                read -p "ip : " ip
                sudo fail2ban-client set <jail_name> unbanip ${ip}
                ;; // period
            n)
                break
                ;;
        esac
    else
        read -p "ip : " ip
        sudo fail2ban-client set <jail_name> unbanip ${ip}
    fi

---

# Store command results in variables

Cover with **`**

    #!/bin/bash
    token=`openssl rand -base64 48`
    echo "ADMIN_TOKEN=${token}"

    > ADMIN_TOKEN=/4jzrb0lSNLSmUS6loVtKloqCnCsnlwWZ6lCoWjCI1hSVMi/gJ0/XAokrqoiooII

---

# grep

example data

    123abcABC
    123abc
    123

- String

        #!/bin/bash
        grep '123' <file>
        > 123abcABC
        > 123abc
        > 123

- Regex

💬 `[0-9]*` is same with `[0-9]{1,}`

        #!/bin/bash
        grep '[0-9]*' <file>
        > 123abcABC
        > 123abc
        > 123

- Output only matching strings

        #!/bin/bash
        grep -o '[a-z]*' <file>
        > abc
        > abc

        // Output all strings before C
        grep -o '[^C]*' <file>
        > 123abcAB
        > 123abc
        > 123

- Pipeline

        #!/bin/bash
        cat <file> | grep -o '[A-Z]*'
        > ABC

---

# grep, awk, uniq, sort

Check only the ip that was baned

- example data

        2021-03-07      nextcloud       INFO    Shutdown in progress...
        2021-03-07      sshd            INFO    Removed logfile: '/var/log/auth.log'
        2021-03-08      fail2ban        NOTICE  [nextcloud] Flush ticker(s) with iptables-multiport
        2021-03-08      nextcloud       INFO    Found 123.123.123.123
        2021-03-09      nextcloud       INFO    Found 111.111.111.111
        2021-03-09      nextcloud       INFO    Found 123.123.123.123
        2021-03-09      nextcloud       INFO    Ban 123.123.123.123

- Extract sentences with ip (grep)

        #!/bin/bash
        grep 'Found' <file>

        > 2021-03-08      nextcloud       INFO    Found 123.123.123.123
        > 2021-03-09      nextcloud       INFO    Found 111.111.111.111
        > 2021-03-09      nextcloud       INFO    Found 123.123.123.123

- Extract ip only (awk)

        2021-03-09      nextcloud       INFO    Found 111.111.111.111
        2021-03-09      nextcloud       INFO    Found 111.111.111.111
        2021-03-09      nextcloud       INFO    Found 123.123.123.123
            $1             $2            $3      $4        $5(NF)

        #!/bin/bash
        grep 'Found" <file> | awk '{print $5}'
        > 123.123.123.123
        > 111.111.111.111
        > 123.123.123.123

- sort

        grep 'Found' <file> | awk '{print $5}' | sort
        > 111.111.111.111
        > 123.123.123.123
        > 123.123.123.123

        // Sort in reverse order
        grep 'Found' <file> | awk '{print $5}' | sort -r

        // options
        -n : number
        -nr : number + reverse
        -u : Deduplication

- Deduplication and Counting (uniq)

        grep 'Found' data.txt | awk '{print $5}' | sort | uniq -c
        >     1 111.111.111.111
        >     2 123.123.123.123

---

# wc

Total line count

    example data
    1234
    1234
    1234

    echo "$data" | wc -l
    > 3

---

# sed

Specific Line Output

    example data
    1234
    abcd
    ABCD

    echo "$var" | sed -n 2p
    > abcd

    // Use with Variables
    echo "$var" | sed -n ${line}p

---

# - %%, #\*

Output front and back by keyword

    data="1234abcdABCD"

    echo ${data%%ab*}
    > 1234

    echo ${data#*ab}
    > cdABCD

---

# cut

Output string as delimiter

`-d` : Separator

`-f` : Field

    "a":{"b":{"c":"string"}}

    echo "$var" | cut -d '"' -f 2
    > a
    echo "$var" | cut -d '"' -f 3
    > :{
    echo "$var" | cut -d '"' -f 4
    > b

---

# Divide Commands

    docker run -d --name <name> --restart unless-stopped -v /bw-data:/data -e SIGNUPS_ALLOWED=false -e SHOW_PASSWORD_HINT=false -p 51425:80

    👇

    docker run -d --name <name> --restart unless-stopped \
    -v /bw-data:/data \
    -e SIGNUPS_ALLOWED=false \
    -e SHOW_PASSWORD_HINT=false \
    -p 51425:80

---

# Verification of input values

    ipRegex="^(([0-9]|[1-9][0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])\.){3}([0-9]|[1-9][0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])$"

    read -p "ip [exit] : " ip

    // Repeat until the conditions below are met
    until [[ -z "$ip" || "$ip" =~ ${ipRegex} ]]; do
        echo "$ip : invalid format"
        read -p "ip [exit] : " ip
    done

    [ -z "$ip" ] && exit // Escape if whitesapce

    ~

---

# Save string to file

    #!/bin/bash

    KEY="1234"

    // Save content until you meet EOF
    cat << EOF > dd.txt
    [interface]
    Address = 10.7.0.200
    DNS = 9.9.9.9
    PrivateKey = $KEY
    EOF

    👇 cat dd.txt

    [interface]
    Address = 10.7.0.200
    DNS = 9.9.9.9
    PrivateKey = 1234

---

# Empty content without deleting (Clear log)

    sudo truncate -s 0 /var/log/nginx/access.log

---

# Check Distribution

    grep "Ubuntu" /etc/os-release
    > NAME="Ubuntu"
    > PRETTY_NAME="Ubuntu 20.04.2 LTS"

    👇 another way 👇

    if [ -f /etc/os-release ]; then
            . /etc/os-release
            OS=$NAME
            VER=$VERSION_ID
    fi

    if [ "$OS" != "Ubuntu" ]; then
            echo "This script seems to be running on an unsupported distribution.
            echo "Supported distribution is Ubuntu."
            exit
    fi

example

    linux=`grep "Ubuntu" /etc/os-release`

    if [ -z "$linux" ]; then
        echo "This script seems to be running on an unsupported distribution."
        echo "Supported distribution is Ubuntu."
        exit
    fi

---

# Check root permission

`"$EUID"` returns UID(User id)

or try `id` command

- root : 0
- user : 1000 ~

        #!/bin/bash
        if [ -n "$EUID" ]; then
                echo "This script needs to be run with superuser privileges."
                exit
        fi

---

# Check logged in user

    echo "$USER" or "$USERNAME"
    > ubuntu

---

# Running in background

    nohup bash script.sh 1> /dev/null 2>&1 &

    # If you want to create log file, run it with the command below.
    nohup bash script.sh 1> [filename].[ext] 2>&1 &

    # Stop script
    ps -ef | grep <script_name>
    kill -9 <PID>

---

# etc

- `cron` file path

  > /var/spool/cron/crontabs/[username]
