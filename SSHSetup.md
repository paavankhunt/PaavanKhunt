# SSH Key Setup Guide

## Generate SSH Keys
```sh
ssh-keygen -t rsa -b 4096 -C "khuntpaavan2001@gmail.com" -f ~/.ssh/id_rsa_auth
ssh-keygen -t rsa -b 4096 -C "khuntpaavan2001@gmail.com" -f ~/.ssh/id_rsa_sign
```

## Add SSH Keys to the SSH Agent
```sh
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa_auth
ssh-add ~/.ssh/id_rsa_sign
```

## Add Authentication Key to GitHub
```sh
cat ~/.ssh/id_rsa_auth.pub
```

## Configure SSH for GitHub, GitLab, and Bitbucket
### Edit the SSH config file:
```sh
nano ~/.ssh/config
```

### Add the following configuration:
```sh
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa_auth
  IdentitiesOnly yes

Host gitlab.com
  HostName gitlab.com
  User git
  IdentityFile ~/.ssh/id_rsa_auth
  IdentitiesOnly yes

Host bitbucket.org
  HostName bitbucket.org
  User git
  IdentityFile ~/.ssh/id_rsa_auth
  IdentitiesOnly yes
```

## Test SSH Authentication
```sh
ssh -T git@github.com
ssh -T git@gitlab.com
ssh -T git@bitbucket.org
```

### Expected output:
```
Hi paavankhunt! You've successfully authenticated, but GitHub does not provide shell access.
```

## Configure Git to Sign Commits with SSH
```sh
git config --global gpg.format ssh
git config --global user.signingkey ~/.ssh/id_rsa_sign.pub
git config --global commit.gpgsign true
git config --global user.email "khuntpaavan2001@gmail.com"
```

## Add Signing Key to GitHub
```sh
cat ~/.ssh/id_rsa_sign.pub
```

## Verify SSH Commit Signing
```sh
git commit -S -m "Your signed commit message"
git log --show-signature
```

## Prevent Unsigned Commits Locally
```sh
git config --global commit.gpgsign true
```

---

## Additional Enhancements

### Set Correct Permissions for SSH Keys
```sh
chmod 600 ~/.ssh/id_rsa_auth ~/.ssh/id_rsa_auth.pub
chmod 600 ~/.ssh/id_rsa_sign ~/.ssh/id_rsa_sign.pub
```

### Automatically Load SSH Keys on Startup
Add this to `~/.bashrc`, `~/.zshrc`, or `~/.bash_profile` (Mac):
```sh
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa_auth
ssh-add ~/.ssh/id_rsa_sign
```

### Force SSH for Git Operations
If Git is using HTTPS instead of SSH:
```sh
git config --global url."git@github.com:".insteadOf "https://github.com/"
git config --global url."git@gitlab.com:".insteadOf "https://gitlab.com/"
git config --global url."git@bitbucket.org:".insteadOf "https://bitbucket.org/"
```

### Git Commit Verification on Remote
Check commit verification on GitHub: [GitHub SSH and GPG Keys](https://github.com/settings/keys)

### Prevent Commit Signing for Specific Repos
Disable signing for a specific repository:
```sh
git config --local commit.gpgsign false
```

### Bitbucket and GitLab Commit Signing
GitHub supports SSH commit signing, but Bitbucket and GitLab do **not** (as of now). If signing is required on these platforms, you may need to use a GPG key instead.

