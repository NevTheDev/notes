# ElasticSearch Ubuntu Setup

## Install ElasticSearch

```cmd
curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
```

```cmd
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
```

```cmd
sudo apt update
```

```cmd
sudo apt install elasticsearch
```


## Configure ElasticSearch

Use nano to edit the elasticsearch config file.

```cmd
sudo nano /etc/elasticsearch/elasticsearch.yml
```
### cluster.name

The cluster.name property identifies which cluster a node belongs to, this should be the same for all nodes.

Suggested format:

{company}-{purpose}


### node.name


### network.host



```yaml
cluster.name: {}
node.name: {}
network.host: 0.0.0.0
discovery.seed_hosts: []
cluster.initial_master_nodes: []

```

```cmd
sudo systemctl start elasticsearch
```

```cmd
sudo systemctl enable elasticsearch
```