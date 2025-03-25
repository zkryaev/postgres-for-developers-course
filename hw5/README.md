# Результаты тестирования производительности

## Запрос

```sh
/usr/lib/postgresql/17/bin/pgbench -c 100 -j 4 -T 10 -f ~/workload.sql -n -U postgres -p 5432 -h localhost thai
```

## Режимы `pool_mode`

### **statement**

```
pgbench (17.4 (Debian 17.4-1.pgdg120+2))
transaction type: /home/zakaryaev.m5/workload.sql
scaling factor: 1
query mode: simple
number of clients: 100
number of threads: 4
maximum number of tries: 1
duration: 10 s
number of transactions actually processed: 139510
number of failed transactions: 0 (0.000%)
latency average = 6.633 ms
initial connection time = 795.502 ms
tps = 15076.027441 (without initial connection time)
```

### **transaction**

```
pgbench (17.4 (Debian 17.4-1.pgdg120+2))
transaction type: /home/zakaryaev.m5/workload.sql
scaling factor: 1
query mode: simple
number of clients: 100
number of threads: 4
maximum number of tries: 1
duration: 10 s
number of transactions actually processed: 140678
number of failed transactions: 0 (0.000%)
latency average = 6.609 ms
initial connection time = 755.748 ms
tps = 15129.935058 (without initial connection time)
```

### **session**

```
pgbench (17.4 (Debian 17.4-1.pgdg120+2))
transaction type: /home/zakaryaev.m5/workload.sql
scaling factor: 1
query mode: simple
number of clients: 100
number of threads: 4
maximum number of tries: 1
duration: 10 s
number of transactions actually processed: 141710
number of failed transactions: 0 (0.000%)
latency average = 6.575 ms
initial connection time = 790.553 ms
tps = 15209.581725 (without initial connection time)
```

