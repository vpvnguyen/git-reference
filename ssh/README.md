# ssh

## Managing Multiple SSH Keys With Same Host

### Git config to auto switch accounts based on repo path

https://superuser.com/questions/366649/ssh-config-same-host-but-different-keys-and-usernames

#### Example Scenario
- Repo: github.com
- Separate SSH keys generated for `GITHUB_USER1` and `GITHUB_USER2`
- Under `~/personal/`, use `GITHUB_USER1` creds
- Under `~/work/`, use `GITHUB_USER2` creds

#### Create aliases and assign keys
- In `~/.ssh/config`, create Host aliases and assign appropriate keys
```
# ~/.ssh/config

Host GITHUB_USER1
  Hostname github.com
  User git
  IdentityFile ~/.ssh/GITHUB_USER1_KEY
  IdentitiesOnly yes

Host GITHUB_USER2
  Hostname github.com
  User git
  IdentityFile ~/.ssh/GITHUB_USER2_KEY
  IdentitiesOnly yes
```

#### Assign alias based on path

- In `~/.config/git/`, create a config to use the alias for each user instead of the default user@hostname 
```zsh
touch config.GITHUB_USER1
touch config.GITHUB_USER2
```
```
# ~/.config/git/config.GITHUB_USER1

[url "GITHUB_USER1:"]
    insteadOf = git@github.com:
```
```
# ~/.config/git/config.GITHUB_USER2

[url "GITHUB_USER2:"]
    insteadOf = git@github.com:
```
- Create a `config` file and assign each config to specific paths
```
touch config
```
```
# ~/.config/git/config

[includeIf "gitdir:~/personal/"]
  path = ~/.config/git/config.GITHUB_USER1

[includeIf "gitdir:~/work/"]
  path = ~/.config/git/config.GITHUB_USER2
```

- Now, `~/personal/` will use `GITHUB_USER1` and `~/work/` will use `GITHUB_USER2`