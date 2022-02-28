# Installation

    sudo apt install iptables

# Principle

    INPUT >>>>> Linux >>>>> OUTPUT
    >>>>>>>>>>> FORWARD >>>>>>>>>>

# Commands

    # List
    iptables --list or -L

    # Append a chain
    iptables -A [chain] [match] [target]

    # Insert a chain
    iptalbes -I [chain] [line number] [match] [target]

    # Replace a chain
    iptables -R [chain] [line number] [match] [target]

    # Delete a chain
    iptables -D [chain] [line number]

    # Backup current settings
    iptables-save > [filename].[ext]

    # Restore backup
    iptables-restore < [filename].[ext]

# chain

    INPUT : Packets entering Linux
    OUTPUT : Packets from Linux
    FORWARD : Packets that use Linux as a router

# match

    -s : source ip
    -d : destination ip
    -p : protocol
    -m : match extensions (Extended Packet Matching Module)
    ...

# target

    ACCEPT
    DROP : Block with no error
    REJECT : Block with connection refused
    LOG : Save packet to syslog

# connection tracking

    NEW : New connection
    ESTABLISHED # Part of an existing connection
    RELATED # New connection with existing connection
    INVALID # A connection that does not contain any connection

# Examples

- iptables -A INPUT -p tcp --dport 51425 -j ACCEPT

  > Allow packets entering 51425/tcp

- iptables -I INPUT 5 -p tcp --dport 51425 -j ACCEPT

  > Allow packets entering 51425/tcp on line 5 of INPUT

- iptables -A INPUT -p tcp --dport 51425 -j DROP

  > Disallow packets entering 51425/tcp

- iptables -A INPUT -p tcp --dport 51425 -j REJECT

  > Disallow packets entering 51425/tcp with "Connection refused" error

- iptables -D INPUT 5

  > Delete INPUT 5 chain

- iptables -R INPUT 5 -p tcp --dport 4000 -j ACCEPT

  > Replace INPUT 5 with "-p tcp --dport 4000 -j ACCEPT"

- sudo iptables -I INPUT -p tcp --dport 80 -s 123.123.123.123 -j ACCEPT

  > Allow 123.123.123.123 IP to connect to 80 ports

- iptables -A INPUT -s 123.123.123.123 -j DROP

  > Block 123.123.123.123 IP

# SSH

Always new connection

    target     prot opt source               destination
    ACCEPT     tcp  --  anywhere             anywhere             state NEW tcp dpt:ssh
    -------------------------------------------------------------------------------------
    sudo iptables -I INPUT 8 -m state -p tcp --dport 22 --state NEW -j ACCEPT

# New chain

Making new chain

    sudo iptables -N test
    sudo iptables -A test -s 111.111.111.111 -j REJECT
    sudo iptables -A test -j RETURN
    sudo iptables -L
    > Chain test (0 references)
    > target     prot opt source               destination
    > REJECT     all  --  111.111.111.111      anywhere             reject-with icmp-port-unreachable
    > RETURN     all  --  anywhere             anywhere

Jump

    sudo iptables -I INPUT 1 -j test
    sudo iptables -L
    > Chain INPUT (policy ACCEPT)
    > target     prot opt source               destination
    > test       all  --  anywhere             anywhere
    > ACCEPT     all  --  anywhere             anywhere             state RELATED,ESTABLISHED
    > ...
    > REJECT     all  --  anywhere             anywhere             reject-with icmp-host-prohibited
    >
    >
    > Chain test (1 references)
    > target     prot opt source               destination
    > REJECT     all  --  111.111.111.111      anywhere             reject-with icmp-port-unreachable
    > RETURN     all  --  anywhere             anywhere

# Delete chain

    sudo iptables -D INPUT 1 # Remove Reference
    sudo iptables -D test 1 # Empty test chain
    sudo iptables -X test # Remove test chain

# Attention

The rule that registered first has priority.

Chain must be added to the below range.

        ---------------------------------------------------------------------------------------------------
          ACCEPT     all  --  anywhere             anywhere             state RELATED,ESTABLISHED

                                               ! here !

          REJECT     all  --  anywhere             anywhere             reject-with icmp-host-prohibited
        ---------------------------------------------------------------------------------------------------

    iptables -I INPUT -m state -p all --state ESTABLISHED,RELATED -j ACCEPT
    iptables -I INPUT -m state (-s 0.0.0.0/0) (-d 192.168.0.5/32) -p all --state ESTABLISHED,RELATED -j ACCEPT

# Restore

The settings are initialized on reboot.

Modify the file below.

    /etc/iptables/rules.v4
    /etc/iptables/rules.v6

or use crontab.

    iptables-save > config/iptables.dump

    crontab -e
    > @reboot sudo iptables-restore < config/iptables.dump (filename.ext)

    sudo service cron reload
