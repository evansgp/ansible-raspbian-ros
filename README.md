# ansible-raspbian-ros

Prepare (and update) a raspbian pi to run Robot Operating System.

Assumes a Rasbian Jessie Lite imaged Raspberry Pi 3B+.

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

Install ansible from [GitHub](https://docs.ansible.com/ansible/2.5/installation_guide/intro_installation.html#running-from-source), you need at least version 2.7.

Add an entry to your `/etc/ansible/hosts`:

```
[pi]
ros ansible_host=192.168.3.157
```

## Playbooks

### install-ros.yml

Configures raspbian and install ROS, specifying the SSH identity you want to use (only required the first time):

```bash
ansible-playbook install-ros.yml --extra-vars "ssh_identity_key=~/.ssh/id_rsa.pub"
```

Currently erroring due to the below issue.

### install-workspace.yml

Installs/updates the workspace:

```bash
ansible-playbook install-workspace.yml
```

There are some `--extra-vars` that can be defined when running the playbook:
- `force_build` when set will force the catkin workspace to rebuilt. Useful for dealing with compilation errors when the workspace itself hasn't changed.

## TODO

- maybe split out the workspace into it's own repo?
- make user and workspace directory configurable
- fix/workaround reboot issue, currently causting an error in the `install-ros.yml` playbook and it needs to be killed (or wait for 10mins)
  - it currently never completes as `Shared connection to 192.168.3.157 closed.` is continuously spammed, even though the target has rebooted fine
- hardware specific clock settings