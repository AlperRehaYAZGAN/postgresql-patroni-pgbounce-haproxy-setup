### Postgresql Patroni-Pgbounce-Haproxy Multinode Master Slave docker-compose Project  
This repository aiming prepare production ready one master and one slave production ready docker-compose files.  

You should go to https://pgtune.leopard.in.ua/ for prepare your own configuration file.  

#### Consul Setup  
On Consul machine  
- set hostname of consul and run as docker-compose project.  
```sh
$ cd 0-consul  
$ export CONSUL_PUBLIC_IP=192.168.1.1  
$ docker-compose -f docker-compose-consul.yml up -d  
```

#### For Every Postgres Worker Node  
On machine:  
- You should generate machine dependent config file using https://pgtune.leopard.in.ua/ and paste config info to './1-pg-patroni-node/builder/pgtune-postgres.conf'. After that prepare './1-pg-patroni-node/builder/pg_hba.conf'  
- Secondly you should find variables defined in README.md and change with your network values. (e.g. POSTGRES_REPLICATOR_PASS ...)  
- Finally you could start your postgres node respectively.  
```sh 
$ cd 1-pg-patroni-node  
$ export CONSUL_URL=http://192.168.1.1:8500
$ docker-compose -f docker-compose-patroni-node.yml up -d # wait till ready.
```  

#### Haproxy Loadbalancer  
On Haproxy machine  
- set config file and run as docker-compose project.  
```sh
$ cd 2-lb-haproxy  
$ # edit ./config/haproxy.cfg with pg node network values.
$ docker-compose -f docker-compose-haproxy.yml up -d  
```

You ready to go!