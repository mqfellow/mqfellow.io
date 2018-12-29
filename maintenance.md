### github cleanup credentials

```

```

### Cleanup docker images and container
```
docker ps -a | awk '{print $1}' | xargs docker rm -f
docker images -q | xargs docker rmi -f
docker rmi $(docker ps -q) 

```
