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
