### Disk space Cleanup

```
du / | awk '+$1 >= 10000000 {print}'
du -h / | awk '$1 ~ /G/ {print}'
```


### github cleanup credentials

```
git config --global --unset credential.helper
```

### Cleanup docker images and container
```
docker ps -a | awk '{print $1}' | xargs docker rm -f
docker images -q | xargs docker rmi -f
docker rmi $(docker ps -q) 

```
### MAC Space Cleanup

```
http://osxdaily.com/2012/04/17/find-large-files-in-mac-os-x-search/
```
