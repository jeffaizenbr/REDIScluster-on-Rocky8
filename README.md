# REDIScluster-on-Rocky8

## update packages
```bash
sudo dnf -y update
```
## list modules
```bash
sudo dnf module list redis
```
## install REDIS
```bash
sudo dnf -y module reset redis && sudo dnf install @redis:6
```
## Configure authentication
```bash
requirepass  <AuthPassword>
```
## Setting Persistent Store for Recovery
```bash
appendonly yes
appendfilename "appendonly.aof"
```
## enable and start redis daemon
```bash
systemctl enable --now redis 
```

### connect REDIS cli
```bash
redis-cli
```
### authenticate
```bash
AUTH <AuthPassword>
```
## check info
```bash
INFO
INFO Server
```
## perform REDIS benchmarking
```bash
redis-benchmark -h 127.0.0.1 -p 6379 -n 100000 -c 10
```
## check clients connecteds
```bash
client list 
```
## use redis python and PHP (OPTIONAL)
```bash
sudo yum -y install  python-redis php-redis
```

# Use Redis as CLUSTER mode

node-1 : Primary node 10.0.0.10
node-2 : Replica node 10.0.0.11
node-3 : Replica node 10.0.0.12

## edit configuration file MASTER
 /etc/redis.conf
```bash
bind 0.0.0.0 ::1 
protected-mode no 
```

## edit configuration file SLAVES
```bash
bind 0.0.0.0 ::1
protected-mode no 

replicaof 10.0.0.10 6379
masterauth <AuthPassword>
```
restart the daemon and using MASTER node, check replication
```bash
redis-cli
AUTH  <AuthPassword>
info replication
```

```bash
```

```bash
```

```bash
```

```bash
```

```bash
```

```bash
```

```bash
```

```bash
```

```bash
```

```bash
```

```bash
```

```bash
```

```bash
```

```bash
```

```bash
```

```bash
```

```bash
```

```bash
```

```bash
```

```bash
```

```bash
```
