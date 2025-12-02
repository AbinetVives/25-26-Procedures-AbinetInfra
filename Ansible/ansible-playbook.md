# 🟩 Ansible

### 1. Werk je Ubuntu server bij

```bash
sudo apt update
sudo apt upgrade -y
```

---

### 2. Ansible installeren (officiële methode)

Gebruik officiële instructies:
🔗 [https://docs.ansible.com/ansible/latest/installation_guide/](https://docs.ansible.com/ansible/latest/installation_guide/)

#### 2.1 Vereiste packages

```bash
sudo apt install -y software-properties-common
```

#### 2.2 Ansible repository toevoegen

```bash
sudo add-apt-repository --yes --update ppa:ansible/ansible
```

#### 2.3 Ansible installeren

```bash
sudo apt install -y ansible
```

#### 2.4 Controleren

```bash
ansible --version
```

---

### 3. Inventory voorbereiden

#### Map aanmaken

```bash
mkdir ~/ansible-project
cd ~/ansible-project
```

#### Inventory bestand maken

```bash
nano inventory.ini
```

---

### 4. SSH-toegang instellen

#### SSH key genereren en kopiëren

```bash
ssh-keygen -t rsa -b 4096
ssh-copy-id ubuntu@SERVER-IP
```

#### Test verbinding

```bash
ansible all -i inventory.ini -m ping
```

---

### 5. Playbook maken

#### Playbook bestand

```bash
nano playbook.yml
```

Voorbeeld playbook:

```yaml
- hosts: web
  become: yes
  tasks:
    - name: Update system
      apt:
        update_cache: yes
        upgrade: yes
```

#### Controleren (dry-run)

```bash
ansible-playbook -i inventory.ini playbook.yml --check
```

---

### 6. Playbook uitvoeren

```bash
ansible-playbook -i inventory.ini playbook.yml
```

---

### 7. Rollenstructuur

Gebruik Ansible Galaxy:

```bash
ansible-galaxy init webserver
```

Dit maakt:

```
webserver/
  tasks/
  handlers/
  templates/
  files/
  vars/
  defaults/
  meta/
```

---

### 8. Variabelen beheren (.env equivalent)

Gebruik:

* `group_vars/`
* `host_vars/`

Voorbeeld:

```
group_vars/web.yml
```

#### Sensitive data

Gebruik **Ansible Vault**:

```bash
ansible-vault encrypt group_vars/web.yml
```

---

### 9. 🔐 Best practices Ansible

#### Security

* Gebruik Vault voor wachtwoorden en gevoelige gegevens.
* Commit geen keys of plain-text credentials.
* Gebruik SSH keys in plaats van wachtwoorden.

#### Stabiliteit

* Houd playbooks **idempotent** (meerdere keren draaien = geen problemen).
* Gebruik **rollen** voor hergebruik.
* Test altijd met `--check` (dry-run).

#### Onderhoud

* Documenteer alle rollen.
* Gebruik **tags** voor taken:

```yaml
tasks:
  - name: Install NGINX
    apt:
      name: nginx
      state: present
    tags: [web, nginx]
```

* Bouw een duidelijke structuur:

```
/inventories
/playbooks
/roles
```

---
