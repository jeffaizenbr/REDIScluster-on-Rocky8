# 🚀 Redis Replication with Ansible

Automate the deployment of a Redis cluster with **1 master + multiple slaves** using Ansible.

This project provides:
- 🔐 Shared authentication across all nodes  
- 🔁 Automatic replication (master → slaves)  
- ⚡ Fast and reproducible deployment  

---

## 📦 Project Structure

```
.
├── hosts
├── play.yml
└── README.md
```

---

## 🖥️ Inventory

Example `hosts` file:

```ini
[redis_master]
redis01 ansible_host=<SET IP>

[redis_slaves]
redis02 ansible_host=<SET IP>
redis03 ansible_host=<SET IP>

[redis_cluster:children]
redis_master
redis_slaves
```

➕ To add more slaves, simply append new hosts under:

```ini
[redis_slaves]
```

---

## ⚙️ Playbook

The `play.yml` file is responsible for:

- 📥 Installing Redis 8.6  
- 🧠 Configuring the master node  
- 🔗 Setting up slaves for automatic replication  
- 🔑 Applying authentication across all nodes  

The `replicaof` directive automatically uses the master IP from the inventory.

---

## 🔐 Password Configuration

Update the variable in the playbook:

```yaml
vars:
  redis_password: "XXXXXXXXX"
```

👉 Replace `"XXXXXXXXX"` with your desired password.

This password will be used for:
- Client authentication  
- Master-slave communication  

---

## ▶️ Usage

1. ✏️ Edit the `hosts` file with your IPs  
2. 🔑 Set the password in `play.yml`  
3. 🚀 Run the playbook:

```bash
ansible-playbook -i hosts play.yml
```

---

## 🔍 Checking Replication

### 🧠 On the Master

```bash
redis-cli -a <your_password> info replication
```

You should see:
- Connected slaves  
- Synchronization status  

---

### 🔁 On the Slaves

```bash
redis-cli -a <your_password> info replication
```

You should see:

```
master_link_status:up
```

---

## ❤️ Health Check

Run on any node:

```bash
redis-cli -a <your_password> info replication
```

✔️ Everything is working correctly when:
- Master shows connected slaves  
- Slaves show active connection to master  

---

## 🧩 Tips

- Use consistent hostnames (`redis01`, `redis02`, etc.)  
- Ensure network connectivity between nodes (port 6379 open)  
- Consider using **Ansible Vault** to protect the password 🔐  
