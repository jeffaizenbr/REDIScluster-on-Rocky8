# 01-REDIS cluster-on-Rocky8

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

# 02 - Use Redis as CLUSTER mode

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
# 03 - Setup more them one port using REDIS

## copy your main redis.conf to redis-6378.conf them add the lines below 
```bash
port 6378
pidfile /var/run/redis-server-6378.pid
logfile /var/log/redis-server-6378.log
dbfilename dump-6378.rdb
```
## create /etc/systemd/system/redis-6378.service
```bash
[Unit]
Description=Redis persistent port 6378
After=network.target

[Service]
ExecStart=/usr/bin/redis-server /etc/redis-6378.conf --daemonize no --supervised systemd
ExecStop=/usr/libexec/redis-shutdown
Type=notify
User=redis
Group=root
RuntimeDirectory=redis
RuntimeDirectoryMode=0755

PIDFile=/var/run/redis/redis-6378.pid

[Install]
WantedBy=multi-user.target
```

## (Optional) copy database
```bash
cp /var/lib/redis/dump.rdb /var/lib/redis/dump-6378.rdb
```

reload daemon and start 
```bash
systemctl daemon-reload
systemctl enable --now reids-6378.service
```
check ports
```bash
netstat -vaulpan | grep -i redis
```
