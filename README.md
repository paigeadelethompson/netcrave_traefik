# Quickstart
Read more about origin certificates in Cloudflare's documentation: https://developers.cloudflare.com/ssl/origin-configuration/origin-ca/

`certificates.yaml` contains the configuration specifying the names of the origin certificates:

```
  certificates:
    - certFile: /certs/netcrave.network.crt
      keyFile: /certs/netcrave.network.key
```

The directory `certs` is mapped to `/certs` when the container is stood up.

# TODO 
- https://github.com/traefik/traefik/issues/11424
