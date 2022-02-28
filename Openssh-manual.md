# Enable openssh

**Settings** > **Apps** > **Apps & features** > **Manage optional features** > **Add a feature**

**Windows 설정** > **앱** > **선택적 기능** > **OpenSSH 클라이언트** 활성화

---

# ssh.exe

❗ `puttygen and ssh-keygen are incompatible.`

Connect with **Public key**

    ssh -i "[private_key_path]" [username]@[hostname]
    👍 i.e. ssh -i "documents/ppk/vpn.key" ubuntu@123.123.123.123

Connect with **password**

    ssh [username]@[hostname]
    👍 i.e. ssh ubuntu@123.123.123.123

---

# sftp.exe

Connect with server

    sftp -i "[private_key_path]" [username]@[hostname]
    👍 i.e. sftp -i "documents/ppk/vpn.key" ubuntu@123.123.123.123

File Transfer

    put [PC_path] [Remote_path]
    👍 i.e. put documents/abcd.sh abcd.sh

Receive File

    get [Remote_path] [PC_path]
    👍 i.e. get abcd.sh documents/abcd.sh

---

# ssh-keygen.exe

    ssh-keygen (-t) (-b)

`-t` : Algorithms

- rsa (Default)
- dsa
- ecdsa
- ed25519

`-b` : Key size

- **RSA** : 2048/4096 bits (default 2048) > 4096 recommended
- **DSA** : 1024
- **ECDSA** : 256, 384, 521 > 521 recommended
- **ED25519** : No options

---

# puttyGen to OpenSSH

    puttyGen menu > Conversions > export as openssh ~
