# Ansible Debian Playbooks

A collection of **Ansible playbooks for automating common Debian Linux administration tasks**.

This project is designed as a practical Ansible learning and automation project. It demonstrates how Ansible can be used to install packages, create files, configure Nginx, and manage Linux services.

## 🚀 Features

This project currently demonstrates:

* 📦 Package installation
* 🌐 Nginx installation
* ⚙️ Nginx configuration
* 📄 File creation
* 📁 File management
* ▶️ Start services
* ⏹️ Stop services
* 🔄 Service management
* 🚫 Disable services
* 🌐 Network tools installation
* 🖥️ Debian server automation

## 📂 Project Structure

```text
ansible_debain_playbook/
│
├── ansible_nginx/
│   └── Nginx-related Ansible files
│
├── deb.ini
│   └── Ansible inventory
│
├── disable.yaml
│   └── Disable a Linux service
│
├── file.yaml
│   └── File creation and management
│
├── net-tool.yaml
│   └── Network tools/package installation
│
├── nginx.yaml
│   └── Nginx installation and configuration
│
├── start.yaml
│   └── Start a Linux service
│
└── stop.yaml
    └── Stop a Linux service
```

## 🛠️ Requirements

### Control Node

Install Ansible on the machine from which you will run the playbooks.

Check the installation:

```bash
ansible --version
```

### Managed Node

The target Debian machine should have:

* Debian Linux
* SSH enabled
* Python installed
* A user with `sudo` privileges
* Network connectivity from the Ansible control node

---

# 🔐 Inventory

The project uses `deb.ini` as the Ansible inventory file.

Example:

```ini
[debian]
192.168.1.100

[debian:vars]
ansible_user=your_user
```

Modify the inventory according to your Debian server.

Test connectivity:

```bash
ansible -i deb.ini all -m ping
```

Expected result:

```text
SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

---

# 📦 Install Packages

The project includes package installation automation.

For example, network tools can be installed using:

```bash
ansible-playbook -i deb.ini net-tool.yaml
```

This demonstrates how Ansible can automate package installation instead of manually running:

```bash
sudo apt install <package>
```

---

# 🌐 Nginx Automation

The project contains Nginx-related automation through:

```text
nginx.yaml
ansible_nginx/
```

The Nginx playbook can be used to automate the installation and configuration of an Nginx web server.

Run:

```bash
ansible-playbook -i deb.ini nginx.yaml
```

After the playbook completes, verify Nginx:

```bash
systemctl status nginx
```

Check whether Nginx is listening:

```bash
ss -tulpn | grep :80
```

Test the web server:

```bash
curl http://SERVER_IP
```

---

# 📄 File Management

The `file.yaml` playbook demonstrates how Ansible can create and manage files on a Debian server.

Run:

```bash
ansible-playbook -i deb.ini file.yaml
```

Ansible's `file` module can be used for tasks such as:

```yaml
- name: Create a directory
  ansible.builtin.file:
    path: /opt/example
    state: directory
    mode: '0755'
```

Create a file:

```yaml
- name: Create configuration file
  ansible.builtin.file:
    path: /opt/example/config.conf
    state: touch
    mode: '0644'
```

---

# ▶️ Start a Service

The `start.yaml` playbook demonstrates starting a Linux service.

Run:

```bash
ansible-playbook -i deb.ini start.yaml
```

Ansible service example:

```yaml
- name: Start Nginx
  ansible.builtin.service:
    name: nginx
    state: started
```

Verify:

```bash
systemctl status nginx
```

---

# ⏹️ Stop a Service

The `stop.yaml` playbook demonstrates stopping a Linux service.

Run:

```bash
ansible-playbook -i deb.ini stop.yaml
```

Example:

```yaml
- name: Stop Nginx
  ansible.builtin.service:
    name: nginx
    state: stopped
```

Verify:

```bash
systemctl status nginx
```

---

# 🚫 Disable a Service

The `disable.yaml` playbook demonstrates disabling a service from automatically starting during system boot.

Run:

```bash
ansible-playbook -i deb.ini disable.yaml
```

Example:

```yaml
- name: Disable Nginx
  ansible.builtin.service:
    name: nginx
    enabled: false
```

Verify:

```bash
systemctl is-enabled nginx
```

---

# 🔄 Service Management

Ansible can manage Linux services using the `service` module.

| Operation       | Ansible State      |
| --------------- | ------------------ |
| Start           | `state: started`   |
| Stop            | `state: stopped`   |
| Restart         | `state: restarted` |
| Reload          | `state: reloaded`  |
| Enable at boot  | `enabled: true`    |
| Disable at boot | `enabled: false`   |

Example:

```yaml
- name: Manage Nginx
  ansible.builtin.service:
    name: nginx
    state: started
    enabled: true
```

---

# 🔑 Using Sudo / Become

Most system administration operations require root privileges.

Playbooks can use:

```yaml
become: true
```

For example:

```yaml
- name: Configure Debian server
  hosts: debian
  become: true

  tasks:
    - name: Install Nginx
      ansible.builtin.apt:
        name: nginx
        state: present
```

If your user requires a sudo password:

```bash
ansible-playbook -i deb.ini nginx.yaml --ask-become-pass
```

or:

```bash
ansible-playbook -i deb.ini nginx.yaml -K
```

---

# 🧪 Check Mode

Before applying changes, you can use Ansible's check mode:

```bash
ansible-playbook -i deb.ini nginx.yaml --check
```

This allows you to preview changes without actually applying them.

---

# 🔍 Verbose Mode

For troubleshooting:

```bash
ansible-playbook -i deb.ini nginx.yaml -v
```

More detailed output:

```bash
ansible-playbook -i deb.ini nginx.yaml -vvv
```

---

# 🧪 Test All Playbooks

Run the individual playbooks from the project root.

### Network tools

```bash
ansible-playbook -i deb.ini net-tool.yaml
```

### File management

```bash
ansible-playbook -i deb.ini file.yaml
```

### Nginx

```bash
ansible-playbook -i deb.ini nginx.yaml
```

### Start service

```bash
ansible-playbook -i deb.ini start.yaml
```

### Stop service

```bash
ansible-playbook -i deb.ini stop.yaml
```

### Disable service

```bash
ansible-playbook -i deb.ini disable.yaml
```

---

# 🎯 Learning Objectives

This project is useful for learning the fundamentals of Ansible Linux administration.

The main concepts demonstrated are:

```text
Ansible
   │
   ├── Inventory
   │
   ├── Playbooks
   │
   ├── Tasks
   │
   ├── Modules
   │   ├── apt
   │   ├── file
   │   └── service
   │
   ├── Package Management
   │
   ├── File Management
   │
   ├── Service Management
   │
   └── Nginx Automation
```

## 📚 Ansible Modules

The project demonstrates several important Ansible modules:

| Module                     | Purpose                             |
| -------------------------- | ----------------------------------- |
| `ansible.builtin.apt`      | Install/manage Debian packages      |
| `ansible.builtin.file`     | Create and manage files/directories |
| `ansible.builtin.service`  | Start/stop/enable/disable services  |
| `ansible.builtin.copy`     | Copy files to remote hosts          |
| `ansible.builtin.template` | Generate configuration files        |

---

# 🔁 Idempotency

One of the important advantages of Ansible is **idempotency**.

You can execute the same playbook multiple times:

```bash
ansible-playbook -i deb.ini nginx.yaml
```

Ansible will only make changes when the target system does not match the desired state.

For example:

```text
First run  → changed
Second run → ok
```

This makes Ansible useful for repeatable server configuration.

---

# 💡 Example Workflow

A typical workflow for this project is:

```text
1. Configure inventory
        ↓
2. Test SSH connectivity
        ↓
3. Install required packages
        ↓
4. Create files/directories
        ↓
5. Install Nginx
        ↓
6. Configure Nginx
        ↓
7. Start Nginx
        ↓
8. Enable Nginx
        ↓
9. Verify the server
```

Test connectivity:

```bash
ansible -i deb.ini all -m ping
```

Install/configure Nginx:

```bash
ansible-playbook -i deb.ini nginx.yaml
```

Verify:

```bash
systemctl status nginx
```

---

# 🏗️ Future Improvements

Possible improvements for this project:

* [ ] Convert playbooks into Ansible roles
* [ ] Add `site.yaml` as a master playbook
* [ ] Add Nginx templates
* [ ] Add static website deployment
* [ ] Add handlers for Nginx restart/reload
* [ ] Add variables
* [ ] Add `group_vars`
* [ ] Add `host_vars`
* [ ] Add Ansible Vault for secrets
* [ ] Add Ansible Lint
* [ ] Add Molecule testing
* [ ] Add Docker-based Debian testing environment
* [ ] Add GitHub Actions CI

---

# 📌 Project

**Repository:**
https://github.com/keyanskv/ansible_debain_playbook

This project is created for learning and practicing **Ansible automation on Debian Linux**.

## 📜 License

This project is intended for educational and learning purposes.
