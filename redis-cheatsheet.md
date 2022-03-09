# Redis Cheatsheet


### Connect to Redis
```
redis-cli -h [HOSTNAME] -p [PORT] -n [DATABASE_NUMBER]
```


### Redis Command
| Command                | Description                |
|------------------------|----------------------------|
| FLUSHALL               | Flush all keys             |
| KEYS *                 | List all keys              |
| TYPE "KEY_NAME"        | Get Type of Key            |
| LRANGE "KEY_NAME" 0 -1 | Get all value on List Type |
| LLEN "KEY_NAME"        | Get count on list          |
