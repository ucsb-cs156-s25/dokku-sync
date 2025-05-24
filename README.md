# dokku-sync

Based on https://chatgpt.com/share/68311489-be88-8012-a76d-2dcf513c2d3c


# ssh-dokku script

This script is placed in `/cs/faculty/pconrad/bin/ssh-dokku`

```bash
#!/bin/bash

if [[ -z "$SSH_ORIGINAL_COMMAND" ]]; then
  echo "No command provided"
  exit 1
fi



# Get the full original command
full_cmd="$SSH_ORIGINAL_COMMAND"

# Extract the first word (command)
dokku_num="${full_cmd%% *}"

case "$dokku_num" in
    00|01|02|03|04|05|06|07|08|09|10|11|12|13|14|15|16)
    # dokku_num is legal
    ;;
  *)
    echo "Unknown dokku num: $dokku_num"
    exit 1
    ;;
esac



# Extract the rest (arguments)
args="${full_cmd#"$dokku_num"}"
args="${args# }"  # Trim leading space

if [[ -z "$args" ]]; then
  echo "No command provided"
  exit 1
fi

destination=dokku-"$dokku_num".cs.ucsb.edu
echo ssh $destination dokku $args
ssh $destination dokku $args
```

# Creating public/private key pair

```
ssh-keygen -t ed25519 -C "github-actions@cs.ucsb.edu" -f ~/.ssh/github-actions-key
```

The public key is copied into `/cs/faculty/pconrad/.ssh/authorized_keys` as follows:

```
command="/cs/faculty/pconrad/bin/ssh-dokku",no-agent-forwarding,no-port-forwarding,no-X11-forwarding,no-pty ssh-ed25519 KEY-VALUE-GOES-HERE-THIS-IS-NOT-THE-REAL-VALLUE github-actions@cs.ucsb.edu
```

Note that it is prefixed so that it can only be used to run this one command.





