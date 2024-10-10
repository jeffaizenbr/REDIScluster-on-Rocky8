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

```bash
```

```bash
```

```bash
```

```bash
```
