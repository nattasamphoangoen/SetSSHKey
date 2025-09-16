# Info | Recommend using Git Bash

```bash
ssh -V
# OpenSSH_6.9p1, LibreSSL 2.1.8
mkdir -p ~/.ssh
cd ~/.ssh

```

# SetSSHKey for Ubuntu

```bash
# Generate new SSH Key
ssh-keygen -t rsa -b 4096 -C "Your E-MAIL"

# Start ssh agent
eval "$(ssh-agent -s)"

# Mac OS
ssh-add -K ~/.ssh/id_rsa
# Linux
ssh-add ~/.ssh/id_rsa

# Add new SSH key to Github or Other Providers (public key)
cat ~/.ssh/id_rsa.pub

# Verify
ssh -T git@github.com

# Fixing permission keys
chmod 600 ~/.ssh/id_rsa
chmod 644 ~/.ssh/id_rsa.pub
```

## Test/Verify ว่า SSH เรา connect ไปที่ target ได้ (ตัวอย่าง Github)
```bash
ssh -T git@github.com

# Verify use port
ssh -T -p 2224 -i ~/.ssh/id_rsa_gitea git@localhost

```

# SSH Config use multiple SSH Key

- ใช้ SSH หลายๆ Key เช่น Gitub / Gitlab / Bicbucket และ Hosting แยกกัน
- ถ้าสร้าง id file ไม่เป็นปกติ(id_rsa) ให้สร้าง config file

```bash
~/.ssh/config

# if no hve config flie
mkdir -p ~/.ssh/config
```

- มี 2 account ใช้ github ผมก็จะแยกแบบนี้

```bash
Host github-local
  HostName github.com
  User git
  IdentityFile ~/.ssh/github

Host github-work
  HostName github.com
  User git
  IdentityFile ~/.ssh/github_work

Host localhost
  HostName localhost
  Port 222
  User git
  IdentityFile ~/.ssh/id_rsa_gitea

```

- กรณี เครือข่าย/องค์กร “บล็อกพอร์ต 22” ทำให้ใช้ SSH ไม่ได้
- ผู้ให้บริการเลยมี “ช่องสำรองผ่านพอร์ต 443” (พอร์ตเดียวกับ HTTPS) โดยเปิด sshd ไว้อีก “โฮสต์พิเศษ”
  - Bitbucket Cloud → altssh.bitbucket.org:443
    (โฮสต์ปกติ bitbucket.org:443 เป็นเว็บ HTTPS ไม่ใช่ SSH — ถ้า SSH ไปที่นี่จะเจอ error “invalid format/baaner” )
  - GitHub → ssh.github.com:443
(แยกจาก github.com:443 ที่เป็นเว็บ)
- ดังนั้นจึงต้อง “ใส่ altssh.” (Bitbucket) และ “ใส่ ssh.” (GitHub) เพื่อให้ SSH client ไปคุยกับพอร์ต 443 ที่เปิดบริการ SSH จริง ๆ ไม่ใช่เว็บ
```bash
# แม็ปผ่าน ~/.ssh/config
# Bitbucket: ใช้ git@bitbucket.org ตามปกติ แต่ให้ไป altssh:443
Host bitbucket.org
  HostName altssh.bitbucket.org
  Port 443
  User git
  IdentitiesOnly yes
  IdentityFile ~/.ssh/id_rsa_bit
  StrictHostKeyChecking accept-new

# GitHub: ใช้ git@github.com ตามปกติ แต่ให้ไป ssh.github.com:443
Host github.com
  HostName ssh.github.com
  Port 443
  User git
  IdentitiesOnly yes
  IdentityFile ~/.ssh/id_ed25519
  StrictHostKeyChecking accept-new

```
