# Redis Replication with Ansible

This repository contains an Ansible playbook to deploy a Redis master with one or more slave nodes.  
It sets up replication automatically and ensures all nodes share the same authentication password.

---

## Inventory

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

Add more slaves by appending them under [redis_slaves].

Playbook
The playbook (play.yml) installs Redis 8.6, configures the master, and sets up slaves to replicate from the master.
The replicaof directive automatically uses the master’s IP from the inventory.

Password Variable
Update the password in the playbook:

yaml
vars:
  redis_password: "XXXXXXXXX"
Replace "XXXXXXXXX" with your desired Redis password.
All nodes will use this password for authentication and replication.

Usage
Edit hosts to include your master and slaves.

Update the password in play.yml.

Run:

```bash
ansible-playbook -i hosts play.yml
Check replication:
```
```bash
redis-cli -a <your_password> info replication
Health Check
Master: shows connected slaves and their status.
```
```bash
redis-cli -a <your_password> info replication
Slave: shows connection to master (master_link_status:up).
```
```bash
redis-cli -a <your_password> info replication
```
