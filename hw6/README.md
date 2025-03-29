# Результаты тестирования с сингл инстансом
### **Сингл инстанс**
**запись**
```
postgres@postgre1:/home/zakaryaev.m5$ /usr/lib/postgresql/17/bin/pgbench -c 8 -j 4 -T 10 -f ~/workload2.sql -n -U postgres -p 5432 thai
pgbench (17.4 (Debian 17.4-1.pgdg120+2))
transaction type: /var/lib/postgresql/workload2.sql
scaling factor: 1
query mode: simple
number of clients: 8
number of threads: 4
maximum number of tries: 1
duration: 10 s
number of transactions actually processed: 49253
number of failed transactions: 0 (0.000%)
latency average = 1.624 ms
initial connection time = 11.035 ms
tps = 4926.987986 (without initial connection time)
```
**чтение**
```
pgbench (17.4 (Debian 17.4-1.pgdg120+2))
transaction type: /var/lib/postgresql/workload.sql
scaling factor: 1
query mode: simple
number of clients: 8
number of threads: 4
maximum number of tries: 1
duration: 10 s
number of transactions actually processed: 284913
number of failed transactions: 0 (0.000%)
latency average = 0.281 ms
initial connection time = 11.052 ms
tps = 28491.157544 (without initial connection time)
```
### **C репликой**
**запись**
```
postgres@postgre1:/home/zakaryaev.m5$ /usr/lib/postgresql/17/bin/pgbench -c 8 -j 4 -T 10 -f ~/workload2.sql -n -U postgres -p 5432 thai
pgbench (17.4 (Debian 17.4-1.pgdg120+2))
transaction type: /var/lib/postgresql/workload2.sql
scaling factor: 1
query mode: simple
number of clients: 8
number of threads: 4
maximum number of tries: 1
duration: 10 s
number of transactions actually processed: 42728
number of failed transactions: 0 (0.000%)
latency average = 1.872 ms
initial connection time = 8.725 ms
tps = 4273.894972 (without initial connection time)
```
**чтение**
```
postgres@postgre1:/home/zakaryaev.m5$ /usr/lib/postgresql/17/bin/pgbench -c 8 -j 4 -T 10 -f ~/workload.sql -n -U postgres thai
pgbench (17.4 (Debian 17.4-1.pgdg120+2))
transaction type: /var/lib/postgresql/workload.sql
scaling factor: 1
query mode: simple
number of clients: 8
number of threads: 4
maximum number of tries: 1
duration: 10 s
number of transactions actually processed: 272426
number of failed transactions: 0 (0.000%)
latency average = 0.294 ms
initial connection time = 9.756 ms
tps = 27250.412693 (without initial connection time)
```
