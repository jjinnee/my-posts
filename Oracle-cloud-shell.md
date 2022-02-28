![](https://images.velog.io/images/jhj46456/post/a3583d5a-6fee-4bdf-91b4-62a5f93e46d3/one.png)

Generate SSH key pairs

    <talent>@cloudshell:~ (ap-seoul-1)$ ssh-keygen
    Generating public/private rsa key pair.
    Enter file in which to save the key (/home/<talent>/.ssh/id_rsa):
    /home/<talent>/.ssh/id_rsa already exists.
    Overwrite (y/n)? y
    Enter passphrase (empty for no passphrase):
    Enter same passphrase again:
    Your identification has been saved in /home/<talent>/.ssh/id_rsa.
    Your public key has been saved in /home/<talent>/.ssh/id_rsa.pub.
    The key fingerprint is:
    SHA256:BzbBxS...fe0b4
    The key's randomart image is:
    +---[RSA 2048]----+
    |    .....o..     |
    |  .=... +.o      |
    | .+oE  .++       |
    |. .. + ..o       |
    | + .+ + S .      |
    |++=o +   .       |
    |o.O=. o          |
    |.*oO.o           |
    |++Xoo            |
    +----[SHA256]-----+
    <talent>@cloudshell:~ (ap-seoul-1)$

After generate SSH key pairs

    <talent>@cloudshell:~ (ap-seoul-1)$ cat .ssh/id_rsa.pub
    ssh-rsa AAAAB3NzaC1y...@5b60e5ffe0b4

Copy public key.

![](https://images.velog.io/images/jhj46456/post/e6042ed3-297a-45b1-9c87-444ca0935b93/two.png)

Create console connection

![](https://images.velog.io/images/jhj46456/post/d3bd8aa8-7cc8-4c29-aafc-7c1601f4e277/three.png)

When the status changes to Active, type the command.

    <talent>@cloudshell:~ (ap-seoul-1)$ ssh -o ProxyCommand='ssh -W %h:%p -p 443 ocid1.instanceconsoleconnection.oc1...a3t2x2x7rpbomyfz42cjydxcjkgjnf4cfq

    =================================================
    IMPORTANT: Use a console connection to troubleshoot a malfunctioning instance. For normal operations, you should connect to the instance using a Secure Shell (SSH) or Remote Desktop connection. For steps, see https://docs.cloud.oracle.com/iaas/Content/Compute/Tasks/accessinginstance.htm

    For more information about troubleshooting your instance using a console connection, see the documentation: https://docs.cloud.oracle.com/en-us/iaas/Content/Compute/References/serialconsole.htm#four
    =================================================
    Enter passphrase for key '/home/<talent>/.ssh/id_rsa':
    Enter passphrase for key '/home/<talent>/.ssh/id_rsa':

    <instance name> login: ubuntu
    Password:


    Login incorrect
    <instance name> login: ubuntu
    Password:
    Welcome to Ubuntu 20.04.2 LTS (GNU/Linux 5.4.0-1038-oracle x86_64)

    ...

    Last login: Sun Mar 14 06:27:37 UTC 2021 from <IP> on pts/0
    You have mail.
    ubuntu@<instance name>:~$

Reboot after edit `.ssh/authorized_keys`
