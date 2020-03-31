# Bird

## Docker

```
docker build -f bird/Dockerfile -t acorso/bird .
```

```
docker run -d --name bird --network host -p 179 \
    -v /tmp/bird/conf:/usr/local/etc/bird:rw \
    -v /tmp/volume/bird/socket:/usr/local/var/run:rw \
    acorso/bird \
    -c /usr/local/etc/bird/bird.conf \
    -s /usr/local/var/run/bird.ctl \
    -d 
```

Check if there is some issues:
```
docker logs bird
```
It should be :`bird: Started`

## Commands

Show protocol
```
docker exec -it bird birdc show protocol
```

Reload the configuration
```
docker exec -it bird birdc configure soft
```