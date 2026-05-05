 🚀 Redis Replication with Ansible

Automatize a criação de um cluster Redis com **1 master + múltiplos slaves** usando Ansible.

Este projeto configura:
- 🔐 Autenticação compartilhada entre todos os nós  
- 🔁 Replicação automática (master → slaves)  
- ⚡ Deploy rápido e reprodutível  

---

## 📦 Estrutura do Projeto

```
.
├── hosts
├── play.yml
└── README.md
```

---

## 🖥️ Inventory

Exemplo de arquivo `hosts`:

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

➕ Para adicionar mais slaves, basta incluir novos hosts em:

```ini
[redis_slaves]
```

---

## ⚙️ Playbook

O arquivo `play.yml` é responsável por:

- 📥 Instalar o Redis 8.6  
- 🧠 Configurar o nó master  
- 🔗 Configurar os slaves para replicação automática  
- 🔑 Aplicar autenticação em todos os nós  

A diretiva `replicaof` usa automaticamente o IP do master definido no inventory.

---

## 🔐 Configuração de Senha

Edite a variável no playbook:

```yaml
vars:
  redis_password: "XXXXXXXXX"
```

👉 Substitua "XXXXXXXXX" pela senha desejada.

Essa senha será usada para:
- Autenticação de clientes  
- Comunicação entre master e slaves  

---

## ▶️ Como Usar

1. ✏️ Edite o arquivo `hosts` com seus IPs  
2. 🔑 Configure a senha no `play.yml`  
3. 🚀 Execute o playbook:

```bash
ansible-playbook -i hosts play.yml
```

---

## 🔍 Verificando Replicação

### 🧠 No Master

```bash
redis-cli -a <your_password> info replication
```

Você deve ver:
- Lista de slaves conectados  
- Status de sincronização  

---

### 🔁 Nos Slaves

```bash
redis-cli -a <your_password> info replication
```

Você deve ver:

```
master_link_status:up
```

---

## ❤️ Health Check

Execute o comando em qualquer nó:

```bash
redis-cli -a <your_password> info replication
```

✔️ Tudo funcionando corretamente quando:
- Master mostra slaves conectados  
- Slaves mostram conexão ativa com o master  

---

## 🧩 Dicas

- Use nomes consistentes para os hosts (`redis01`, `redis02`, etc.)  
- Garanta conectividade entre os nós (porta 6379 liberada)  
- Considere usar **Ansible Vault** para proteger a senha 🔐  
