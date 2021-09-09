# Installing Neo4j in WSL2 Ubuntu 20.04 LTS

## Install java

```console
sudo apt install openjdk-11-jre-headless
```
## Add neo4j repo
Basically we can follow the instructions for debian install in the Operations Manual. However initd is used since systemd is not available in WSL2.  
```console
wget -O - https://debian.neo4j.com/neotechnology.gpg.key | sudo apt-key add -
echo 'deb https://debian.neo4j.com stable latest' | sudo tee -a /etc/apt/sources.list.d/neo4j.list
sudo apt-get update
```

## Show available versions
```console
apt list -a neo4j
```

## Install 
```console
sudo apt-get install neo4j-enterprise=1:4.3.3
```

## Start
Since WSL2 does not have systemd, but initd
```console
sudo service neo4j start
```


## Default locations

```console
home:         /var/lib/neo4j
config:       /etc/neo4j
logs:         /var/log/neo4j
plugins:      /var/lib/neo4j/plugins
import:       /var/lib/neo4j/import
data:         /var/lib/neo4j/data
certificates: /var/lib/neo4j/certificates
licenses:     /var/lib/neo4j/licenses
run:          /var/run/neo4j
```

## Config
To access the database from host (outside wsl2) 
```properties
dbms.default_listen_address=0.0.0.0
```

And we should always specify heap
```properties
dbms.memory.heap.initial_size=2g
dbms.memory.heap.max_size=2g
```


## APOC (core)
Copy the included apoc jar file form to the plugins folder
```console
sudo cp  /var/lib/neo4j/labs/apoc*core.jar /var/lib/neo4j/plugins/.
```


## APOC (full)
Get the right version from github
```console
sudo wget https://github.com/neo4j-contrib/neo4j-apoc-procedures/releases/download/4.3.0.1/apoc-4.3.0.1-all.jar -O /var/lib/neo4j/plugins/apoc-full.jar
```

Remeber to restart after config change or adding/removing plugins
```console
sudo service neo4j restart
```


## Remarks

1. Since we did not specify auth, the first time
log in with neo4j/neo4j

2. The database keeps running even if you close the wsl terminal window

## Todo

1. Check shutdown behaviour when powering off windows

