# Selenium Grid

## Basic run (1 node of chrome, firefox and edge)

``` cmd
docker-compose up -d
```

## Scaling the containers ()

``` cmd
docker-compose up -d --scale chrome=10 --scale firefox=5

docker-compose up -d --scale chrome=10 --scale firefox=10 --scale edge=10

```