cat /etc/ssh/sshd_config

nano /etc/ssh/sshd_config

    Port <number>
    PermitRootLogin prohibit-password
    PubkeyAuthentication yes
    AuthorizedKeysFile     .ssh/authorized_keys .ssh/authorized_keys2
    PasswordAuthentication no
    PermitEmptyPasswords no

sudo service ssh(d) restart
