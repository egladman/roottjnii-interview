# roottjnii-interview

## Objective

Deploy the provided docker image on Fedora with Ansible.

## Work Prompt

The Mauve team is ready to deploy their app and needs your help deploying it. They have
prepared the following Docker container that listens for HTTP requests on port 4567:

```
roottjnii/interview-container:201805
```

Using your configuration management method of choice create a
manifest/recipe/module/playbook to allow us to deploy this container to multiple servers and
environments. You may target whichever OS distro you prefer. After provisioning the target
server should be running the container with container port 4567 exposed to the network. After
the container is deployed “curl http://[hostip]:4567” should return data. Also, this is an alpha
version of the app and it has a bug which causes it to occasionally crash. Please ensure the
container automatically restarts. You are not expected to troubleshoot the crash at this time.


## Development - Fedora

### Install Dependencies

```
sudo dnf install ansible
```

**Note:** Ansible runs on Python `v2.6` and higher. Python 3 is the default interpreter and comes preinstalled. As of writing this `v3.7.5` is shipped with Fedora 31.


## Deployment - Fedora

### Locally - *Tested*

```
sudo dnf install ansible

git clone git@github.com:egladman/roottjnii-interview.git
cd roottjnii-interview
ansible-playbook --ask-become-pass --connection=local --inventory 127.0.0.1, site.yml
```

**NOTE:** Depending on how your user is configured you might need to run `ansible-playbook` as followed:
```
sudo ansible-playbook --connection=local --inventory 127.0.0.1, site.yml
```


### Remotely

**NOTE:** You must update `hosts` with valid servers before proceeding
```
git clone git@github.com:egladman/roottjnii-interview.git
cd roottjnii-interview
ansible-playbook --ask-become-pass --inventory development site.yml
```

### Validation

```
curl <host>:4567
```

## Things to Consider

1. In it's current state, the container runs as in unpriveleged user, but they're a member of group: `docker`. This has [security implications](https://docs.docker.com/engine/security/security/#docker-daemon-attack-surface). If I were to continue working on this project I'd consider [running the docker daemon rootless](https://docs.docker.com/engine/security/rootless/) (currently experimental). 
2. Directory structure is based off of [Playbook Best Practices](https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html#content-organization).
3. Only Docker Community Edition is supported at this time.
4. The target host will reboot during the execution of `ansible-playbook` if changes to the kernel are detected, this is by design.
