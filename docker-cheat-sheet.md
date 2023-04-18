# Docker Cheat Sheet

## Docker Logs

### Docker Service Logs With Colour Timestamp
```
docker service logs DOCKER-SERVICE-NAME -t --tail 100 -f 2>&1 | sed -r 's/^([^Z]+Z )/\x1b[33m\1\x1b[0m /gm'
```

### Docker Service Logs Find String Using Grep
```
docker service logs DOCKER-SERVICE-NAME -t --tail 100 -f 2>&1 | grep FIND-STRING
```

## Docker Stack
### Docker Stack Deploy with Environment Variable
```
docker stack deploy \
  -c <(docker-compose -f YOUR-DOCKER-COMPOSE-FILE.yml --env-file YOUR-DOCKER-COMPOSE-ENVIRONMENT-FILE.env config) \
  --with-registry-auth YOUR-SERVICE-NAME
```
## Docker Service
### Get Running Docker Service Env 
```
docker service inspect --format='{{range .Spec.TaskTemplate.ContainerSpec.Env}}{{println .}}{{end}}' YOUR-SERVICE-NAME
```

### Docker Service Find Running Container Location
```
docker service ps -f desired-state=running YOUR-SERVICE-NAME
```

### Get Update Status
```
docker service inspect --format='{{json .UpdateStatus}}' YOUR-SERVICE-NAME
```

## Docker Network
### Docker Get IP Address From Container on Docker Swarm
```
docker network inspect docker_gwbridge | \
  grep -A 5 $(docker ps -f "name=YOUR_SERVICE_NAME" -q | \
  head -n 1) | grep IPv4 | awk -v RS='([0-9]+\\.){3}[0-9]+' 'RT{print RT}'
```

## Docker Node
### Get Docker Node List With Label
```
docker node ls -q | xargs docker node inspect \
  -f '{{ .ID }} [{{ .Description.Hostname }}]: {{ range $k, $v := .Spec.Labels }}{{ $k }}={{ $v }} {{end}}'
```

## Docker Daemon
### Docker clean up storage
```
docker image rm $(docker images REGISTRY_URL/TEAM_NAME/* -q) -f
sudo sh -c "truncate -s 0 /var/lib/docker/containers/*/*-json.log"
sudo docker builder prune
sudo docker image prune
docker rm $(docker ps -a -f status=exited -q)

docker exec -i $(docker ps -f name=SERVICE_REGISTRY_NAME -q) bin/registry garbage-collect /etc/docker/registry/config.yml
```

### Docker determining container responsible for largest overlay directories
```
# as root
sudo su

# grab the size and path to the largest overlay dir
du /var/lib/docker/overlay2 -h | sort -h | tail -n 100 | grep -vE "overlay2$" > large-overlay.txt

# make sure json parser is installed 
apt-get install jq -y 

# construct mappings of name to hash
docker inspect $(docker ps -qa) | jq -r 'map([.Name, .GraphDriver.Data.MergedDir]) | .[] | "\(.[0])\t\(.[1])"' > docker-mappings.txt

# for each hashed path, find matching container name
cat large-overlay.txt | xargs -l bash -c 'if grep $1 docker-mappings.txt; then echo -n "$0 "; fi'
```
