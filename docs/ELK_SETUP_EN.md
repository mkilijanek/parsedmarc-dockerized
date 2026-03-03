# ELK Setup (EN)

This document describes how to run the local ELK stack from `docker-compose-elk.yml`.
The stack creates the `elasticsearch_elastic` Docker network used by `parsedmarc` from the main `docker-compose.yml`.

## What `docker-compose-elk.yml` starts
- `elasticsearch` (single-node, port `9200`)
- `kibana` (port `5601`)
- optional `nginx` (profile `nginx`, ports `80` and `443`)

## Start ELK
```bash
sudo docker compose -f docker-compose-elk.yml up -d
```

## Start ELK + Nginx
```bash
sudo docker compose -f docker-compose-elk.yml --profile nginx up -d
```

## Check status
```bash
sudo docker compose -f docker-compose-elk.yml ps
sudo docker compose -f docker-compose-elk.yml logs -f elasticsearch kibana
```

## Integration with parsedmarc
After ELK is up, start parser separately:
```bash
sudo docker compose up -d parsedmarc
```

In `parsedmarc/parsedmarc.ini`, Elasticsearch host should point to the container in that network, e.g.:
```ini
[elasticsearch]
hosts = es01:9200
ssl = False
```

Note: if you use TLS/secured Elasticsearch, adjust `hosts`, `ssl`, `user`, and `password` for your environment.

## Stop
```bash
sudo docker compose -f docker-compose-elk.yml down
```

## Remove Elasticsearch data
```bash
sudo docker compose -f docker-compose-elk.yml down -v
```
This removes the `esdata01` volume.
