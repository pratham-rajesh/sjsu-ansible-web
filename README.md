# HW #1 - Ansible

This repo lets you:
- Launch **two Ubuntu VMs** on a Mac M‑series using **Multipass**
- Use **Ansible** to deploy a web server on **port 8080** to both VMs
- Each VM shows a different message: `Hello World from SJSU-1` and `Hello World from SJSU-2`
- Include **deploy** and **undeploy** plays in one `site.yml`


## 1) Create two VMs with Multipass

```bash
multipass launch 22.04 --name vm1 --cpus 2 --memory 2G --disk 10G
multipass launch 22.04 --name vm2 --cpus 2 --memory 2G --disk 10G
multipass list
```

Copy the IP addresses of `vm1` and `vm2`. Update `inventory.ini` accordingly.

> Default username in Multipass is `ubuntu`. Passwordless SSH uses your Apple account keychain automatically; if needed you can `multipass exec vm1 -- bash` to set things up.

## 2) (Optional) Enable passwordless SSH for Ansible

```bash
# Generate a key if you don't have one
ssh-keygen -t ed25519 -C "sjsu"

# Get IPs 
multipass list

# Copy your key into both VMs
ssh-copy-id ubuntu@192.168.64.5
ssh-copy-id ubuntu@192.168.64.6
```

## 3) Test Ansible connectivity

```bash
ansible -m ping web
```

You should see `pong` from both hosts.

## 4) Deploy

```bash
ansible-playbook site.yml --tags deploy
```

Verify in a browser:
- `http://192.168.64.5:8080` shows **Hello World from SJSU-1**
- `http://192.168.64.6:8080` shows **Hello World from SJSU-2**

## 5) Undeploy

```bash
ansible-playbook site.yml --tags undeploy
```

