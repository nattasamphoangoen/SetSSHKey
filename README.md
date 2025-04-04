# into

```bash
ssh -V
# OpenSSH_6.9p1, LibreSSL 2.1.8
mkdir -p ~/.ssh
cd ~/.ssh

```

# SetSSHKey for Ubuntu

```bash
# Generate new SSH Key
ssh-keygen -t rsa -b 4096 -C "EMAIL"

# Start ssh agent
eval "$(ssh-agent -s)"

# Mac OS
ssh-add -K ~/.ssh/id_rsa
# Linux
ssh-add ~/.ssh/id_rsa

# Add new SSH key to Github or Other Providers (public key)
cat ~/.ssh.id_rsa.pub

# Verify
ssh -T git@github.com

# Fixing permission keys
chmod 600 ~/.ssh/id_rsa
chmod 644 ~/.ssh/id_rsa.pub
```

# SSH Config

- ใช้ SSH หลายๆ Key เช่น Gitub / Gitlab / Bicbucket และ Hosting แยกกัน
- ถ้าสร้าง id file ไม่เป็นปกติ(id_rsa) ให้สร้าง config file

```bash
~/.ssh/config
```

-- มี 2 account ใช้ github ผมก็จะแยกแบบนี้

```bash
Host github.com
  User git
  IdentityFile ~/.ssh/github

Host github.com-work
  User git
  IdentityFile ~/.ssh/github_work
```
