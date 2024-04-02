# SSH Configuration

DLSS developers tend to use many of the same settings when we connect to hosts via the SSH protocol. Here we document the most common settings, which are typically stored in `~/.ssh/config`. These ought to be usable across platforms, and have been tested on modern (as of 2024) releases of both MacOS and Ubuntu Linux.

Things you'll need to know before applying the configuration below:

* Your SUNet ID
* The path to the private SSH key you will use to connect to Stanford hosts
* The hostname for the jump/bastion host you will most often use (most likely sdr-infra.stanford.edu or dlss-ci-prod.stanford.edu)

```ssh-config
# ~/.ssh/config

# First, set up a jump/bastion host. You'll generally want your SSH config ordered from more
# specific `Host` entries to less specific ones due to how the configs are applied (first matching).
Host sdr-infra # If your team doesn't use `sdr-infra`, you probably want `dlss-ci-prod` here
    # Start multiplexing if no current master, else use current master
    ControlMaster auto
    # Path to unique socket file based on username, hostname, and port
    ControlPath ~/.ssh/%r@%h:%p
    # How long to keep jump host connection persisted, tune this for your needs
    ControlPersist 1h
    # Host name of jump/bastion host
    HostName sdr-infra.stanford.edu # You might prefer dlss-ci-prod.stanford.edu here.
    # Prevent an endless proxy jump loop
    ProxyJump none

# Next, apply settings for all Stanford-based hosts.
Host *.stanford.edu
    # Allow interacting with e.g. GitHub on servers (pulling shared_configs, etc.)
    ForwardAgent yes
    # Use Kerberos for authentication
    GSSAPIAuthentication yes
    # Specify your Kerberos identity (Linux-only option)
    GSSAPIClientIdentity YOUR_SUNET_ID@stanford.edu
    # Share your Kerberos tickets with the server
    GSSAPIDelegateCredentials yes
    # Set up Kerberos key exchange (Linux-only option)
    GSSAPIKeyExchange yes
    # Set a jump/bastion host for Stanford SSH connections
    ProxyJump sdr-infra # You might prefer dlss-ci-prod here.
    # Set a default username for Stanford SSH connections (no `@stanford.edu`)
    User YOUR_SUNET_ID

# Finally, apply settings to all SSH connections unless overridden above
Host *
    # Ignore platform-specific options. This is intentionally the first setting in the `Host *` block`
    IgnoreUnknown GSSAPIKeyExchange,GSSAPIClientIdentity,UseKeychain
    # Specify domain names to be used by CanonicalizeHostname below
    CanonicalDomains stanford.edu
    # Append a domain name (above) to hostnames w/o domains and re-apply configs above to fully-qualified hostname
    CanonicalizeHostname yes
    # Add SSH keys to the running SSH agent
    AddKeysToAgent yes
    # Store SSH passphrases in the keychain (MacOS-only option)
    UseKeychain yes
    # Path to your private SSH key
    IdentityFile ~/.ssh/id_ed25519 # or e.g. ~/.ssh/id_rsa
```
