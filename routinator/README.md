# Routinator

Source - https://github.com/NLnetLabs/routinator

## Docker

Create a Docker volume to persist TALs and data
```
docker volume create routinator
```

Run a disposable container to install TALs
```
docker run --rm -v routinator:/home/routinator/.rpki-cache \
    nlnetlabs/routinator init -f --accept-arin-rpa
```

Launch the final detached container named 'routinator' 
- RTR port: 3323
- Metrics amd status via http: 9556
```
docker run -d --restart=unless-stopped --network host --name routinator -p 3323 \
    -p 9556 -v routinator:/home/routinator/.rpki-cache \
    nlnetlabs/routinator
```

Check the process
```
docker logs routinator
docker exec -it routinator routinator -v vrps
```

## Supervision
```
curl http://lg.acorso.fr:9556/version
curl http://lg.acorso.fr:9556/status
curl http://lg.acorso.fr:9556/metrics
```
