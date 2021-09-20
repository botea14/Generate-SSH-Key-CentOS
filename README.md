Generate SSH Key (local machine)

`ssh-keygen -t rsa -b 4096`

Create .ssh on VM

`mkdir -p ~/.ssh && touch ~/.ssh/authorized_keys`

`chmod 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys`

`nano ~/.ssh/authorized_keys`
