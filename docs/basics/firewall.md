# Firewall

## Architecture

* Netfilter: Framework in Linux kernel
* nftables: Modern iptables (efficient, clean syntax)
* Frontends:
  * firewalld: Standard on RHEL, Fedora, CentOS
  * UFW (Uncomplicated Firewall): Standard on Ubuntu/Debian, easy to use

## Important concepts

### Chains

1. INPUT: Packages, destined for the server
2. OUTPUT: Packages, send from the server
3. FORWARD: Packages, forwarded by the server

### Stateful Inspection

Modern firewalls are stateful. That means they can check if an incoming package is the answer to a request.

## Commands

```sh
firewall-cmd --get-active-zones # Which zone is active
firewall-cmd --zone=public --add-port=443/tcp --permanent # Opens up ports permanently
firewall-cmd --reload # Reaload config (to activate changes)
```

## Checklist

1. **Default Deny**: Most important. Everything thats not explicitly allowed should be blocked
2. **SSH-Lockout**: Before you activate the firewall you have to check if port 22 is opend. Otherwise you lockout yourself
3. **Logging**: Activate logging for denied packages
