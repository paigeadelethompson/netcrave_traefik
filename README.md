# Quickstart
Read more about origin certificates in Cloudflare's documentation: https://developers.cloudflare.com/ssl/origin-configuration/origin-ca/
Setup origin certificates for the domain you intend to use, and enable the "Authenticated Origin Pulls" option in the Cloudflare dashboard. 

`certificates.yaml` contains the configuration specifying the names of the origin certificates:

```
  certificates:
    - certFile: /certs/netcrave.network.crt
      keyFile: /certs/netcrave.network.key
```

The directory `certs` is mapped to `/certs` when the container is stood up.

If you want to use more than 1 domain, add an additional entry under `certificates`: 

```
    - certFile: /certs/netcrave.chat.crt
      keyFile: /certs/netcrave.chat.key
```

# Adding a backend to Traefik
In your application's `docker-compose.yml` add the top level network element:
```
networks:
  traefik_live:
    external: true
```

add this to the container configuration: 
```
    networks:
      - traefik_live
```

add the following labels to the container configuration: 
```
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.myservice.rule=Host(`myservice.netcrave.io`)"
      - "traefik.http.services.myservice.loadbalancer.server.port=3000"
      - "traefik.http.routers.myservice.entrypoints=web"
      - "traefik.http.routers.myservice.service=myservice"
      - "traefik.http.routers.myservice-https.rule=Host(`myservice.netcrave.io`)"
      - "traefik.http.routers.myservice-https.tls=true"
      - "traefik.http.services.myservice-https.loadbalancer.server.port=3000"
      - "traefik.http.routers.myservice-https.entrypoints=websecure"
      - "traefik.http.routers.myservice-https.service=myservice"
```

- replace `myservice` everywhere with the container name of the application
- change `myservice.netcrave.chat` to the domain that you want to use for the application 
- change `traefik.http.services.myservice.loadbalancer.server.port=3000` to the correct port for the application

When the application is stood up, Traefik will automatically configure it's routes to work with it.

# Troubleshooting
- Edit `config.env` and set `LOG_LEVEL` to `debug`
- Optionally, set `ACCESS_LOG` to `true` for request logging

# TODO 
- https://github.com/traefik/traefik/issues/11424
