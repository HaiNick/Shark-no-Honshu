# ssh remote access because servers need management.

## key generation
```bash
ssh-keygen -t rsa -b 4096                # generate RSA key pair
ssh-keygen -t ed25519                     # generate ed25519 key (recommended)
```

## passwordless auth setup
```bash
ssh-copy-id username@remote_host          # copy public key to remote
ssh-copy-id -i ~/.ssh/custom_key.pub user@host    # specify custom key
```

## connection options
```bash
ssh user@host                            # basic connection
ssh -p 2222 user@host                    # custom port
ssh -i ~/.ssh/custom_key user@host       # specific private key
ssh -L 8080:localhost:80 user@host       # local port forwarding
ssh -R 8080:localhost:80 user@host       # remote port forwarding
ssh -D 1080 user@host                    # SOCKS proxy
```

## config file (`~/.ssh/config`)
```
Host myserver
    HostName 192.168.1.100
    User admin
    Port 2222
    IdentityFile ~/.ssh/myserver_key
```

## troubleshooting
```bash
ssh -v user@host                         # verbose output
ssh -vv user@host                        # more verbose
ssh-add -l                              # list loaded keys
ssh-add ~/.ssh/private_key               # add key to agent
```

## notes
[!] ed25519 keys are smaller and more secure than RSA
[!] always verify host fingerprints on first connection
[!] keep private keys secure - never share them