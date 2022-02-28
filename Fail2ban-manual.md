# Install

    sudo apt install fail2ban

# Filter

- `filter path` : /etc/fail2ban/filter.d/~

Create file

    nano /etc/fail2ban/filter.d/name.conf

Add contents

    [INCLUDES]
    before = common.conf
    after = filtername.local (optional)

    [Definition]
    failregex = <regex>
    ignoreregex =

    // attention
    Apr-07-13 07:08:36 Invalid command fial2ban from 1.2.3.4
    👇
    ^Invalid command \S+ from <HOST>$
    ^Invalid command .* from <HOST>$

    ^.*Username or password is incorrect\. Try again\. IP: <ADDR>\. Username:.*$

# Jail

- `jail path` : /etc/fail2ban/jail.d/~

Create file

    nano /etc/fail2ban/jail.d/name.local

Add contents

    [service_name]
    enabled = true
    ignoreip = <your_ip>/32 (optional)
    port = <service_port> i.e. 51425
    filter = <filter_name>
    action = <Check /etc/fail2ban/jail.conf>
    logpath = <Path_where_logs_are_stored> i.e. /bw-data/bitwarden.log
    maxretry = 5
    bantime = 14400
    findtime = 14400

# Command

    fail2ban-client status (jail_name)

    fail2ban-client set [jail_name] banip 123.123.123.123

    fail2ban-client set [jail_name] unbanip 123.123.123.123

    fail2ban-client restart

    fail2ban-client reload

    cat /var/log/fail2ban.log\* | grep "] Ban" | awk '{print $NF}' | sort | uniq -c | sort -n
