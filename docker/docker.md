# docker



### Getting images

``` cmd
docker pull mcr.microsoft.com/dotnet/runtime:5.0
```

``` cmd
docker run 
```


``` cmd
docker run -d -p 80:80 my_image nginx -g 'daemon off;'
```

``` cmd
docker pull elasticsearch
```

``` cmd
docker run -d  --name elastic1 -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:7.10.1
```

```
docker run -d --name my-redis -p 6379:6379 redis
```

docker pull mcr.microsoft.com/mssql/server:2019-latest

docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=yourStrong(!)Password" -e "MSSQL_PID=Express" -p 1433:1433 -d mcr.microsoft.com/mssql/server:2017-latest-ubuntu 

## build an image

```
docker build -t my-app -f Dockerfile .

docker build -t my-app:feature-499 -f Dockerfile .
```


## Create a container

``` 
docker create --name my-app 