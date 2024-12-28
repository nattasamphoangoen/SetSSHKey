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