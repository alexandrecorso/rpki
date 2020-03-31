# Octorpki

Source - https://github.com/cloudflare/cfrpki

## Keys

Private
```
openssl ecparam -genkey -name prime256v1 -noout -outform pem > /srv/volumes/octorpki/private.pem
```

Public
```
openssl ec -in /srv/volumes/octorpki/private.pem -pubout -outform pem > /srv/volumes/octorpki/public.pem
```

## Docker

Create a repository to persist TALs and data
```
mkdir -p /srv/volumes/octorpki/{tals,cache}
touch /srv/volumes/octorpki/rrdp.json
```

Import all tals (https://github.com/alexandrecorso/rpki/tree/master/tals)
```
git clone https://github.com/alexandrecorso/rpki /tmp/rpki
cp /tmp/rpki/tals/{afrinic.tal,apnic.tal,arin.tal,lacnic.tal,ripe.tal} \
    /srv/volumes/octorpki/tals
```

Fix right issue (octorpki use user `rpki=100` group `nogroup=65533`)
```
chmod -R 777 /srv/volumes/octorpki
```
or 
```
chown -R 100:65533 /srv/volumes/octorpki
```

Start the daemon
```
docker run -d --name octorpki --network host -p 9554:9554 \
  -v /srv/volumes/octorpki/private.pem:/private.pem:rw \
  -v /srv/volumes/octorpki/rrdp.json:/rrdp.json:rw \
  -v /srv/volumes/octorpki/tals:/tals:rw \
  -v /srv/volumes/octorpki/cache:/cache:rw \
  cloudflare/octorpki -http.addr :9554 -rsync.timeout 5m -refresh 20m
```

Verify
```
docker logs octorpki
```

## Supervision
```
curl http://lg.acorso.fr:9554/infos
curl http://lg.acorso.fr:9554/metrics
```

## Data
```
curl http://lg.acorso.fr:9554/output.json
```
