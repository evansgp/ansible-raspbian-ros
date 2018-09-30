# ansible-raspbian-ros

Prepare (and update) a raspbian pi to run Robot Operating System.

Uses the following roles:
  - raspbian: updates all packages and optionally copies an SSH identity to the pi user and disables password authentication
  - ros: installs and updates a generic ros system
  - workspace: creates a catkin workspace for my specific robot

## Imaging

Get your raspbian stretch lite imaged pi onto the network, you can do this headless with something like:

```bash
touch bootfs/ssh
vi rootfs/etc/wpa_supplicant/wpa_supplicant.conf
```

## Dependencies

Install ansible:

```bash
pip install ansible
```

Add an entry to your `/etc/ansible/hosts`:

```
[pi]
ros ansible_host=192.168.3.157
```

## Playbooks

### install-ros.yml

Installs the full stack, specifying the SSH identity you want to use (only required the first time):

```bash
ansible-playbook install-ros.yml --extra-vars "ssh_identity_key=~/.ssh/id_rsa.pub"
```

### update-workspace.yml

Installs/updates the workspace only:

```bash
ansible-playbook update-workspace.yml
```

## TODO

- maybe split out the workspace into it's own repo?
- make user and workspace directory configurable
