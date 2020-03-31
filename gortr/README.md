# GoRTR

Source - https://github.com/cloudflare/gortr

## Pre-requis

- Octorpki
- Private key

## Docker

- RTR port: 8282
- RTR ssh: 8283
- Metrics via http: 9552
```
docker run -d --name gortr --network host -p 8282 -p 8283 -p 9552 \
  -v /srv/volumes/octorpki/private.pem:/private.pem:rw \
  cloudflare/gortr \
  -bind :8282 \
  -ssh.bind :8283 \
  -metrics.addr :9552 \
  -ssh.key=private.pem -ssh.auth.user=rpki \
  -ssh.method.key=true -ssh.auth.key.bypass=true \
  -verify=false -checktime=false \
  -cache="http://lg.acorso.fr:9554/output.json"
```

## Supervision

```
curl http://lg.acorso.fr:8080/metrics
```