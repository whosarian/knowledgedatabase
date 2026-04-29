# SSH (Secure Shell)

SSH is a protocol framework for encrypted communication, that unites authentication, encryption and integrity.

## Key concepts

SSH uses an asymmetric encryption for authentication and a symmetric encryption for a running session.

- Public key: stored on the target server (`authorized_keys`)
- Private key: has to be a secret, ideally protected with a passphrase

## Important files and paths

| Path                     | Usage                                                                  |
| ------------------------ | ---------------------------------------------------------------------- |
| `~/.ssh/id_rsa`          | Private key                                                            |
| `~/.ssh/id_rsa.pub`      | Public key                                                             |
| `~/.ssh/authorized_keys` | List of public keys, that can log in a account                         |
| `~/.ssh/known_hosts`     | Directory of fingerprints of hosts to which you where connected before |
| `/etc/ssh/sshd_config`   | Configuration file for the SSH-daemon                                  |

## SSH-agent and forwarding

The SSH-agent is a program that holds your private keys in the storage, so you don't have to type your passphrase every time.

With agent forwarding you are able to use your local keys on a remote host to jump to another one without copying your private key on the host.
