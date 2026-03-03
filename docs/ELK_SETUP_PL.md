# Konfiguracja ELK (PL)

Ten dokument opisuje uruchomienie lokalnego stacka ELK z pliku `docker-compose-elk.yml`.
Stack tworzy sieć `elasticsearch_elastic`, do której podłącza się `parsedmarc` z głównego `docker-compose.yml`.

## Co uruchamia `docker-compose-elk.yml`
- `elasticsearch` (single-node, port `9200`)
- `kibana` (port `5601`)
- opcjonalnie `nginx` (profil `nginx`, porty `80` i `443`)

## Start ELK
```bash
sudo docker compose -f docker-compose-elk.yml up -d
```

## Start ELK + Nginx
```bash
sudo docker compose -f docker-compose-elk.yml --profile nginx up -d
```

## Sprawdzenie stanu
```bash
sudo docker compose -f docker-compose-elk.yml ps
sudo docker compose -f docker-compose-elk.yml logs -f elasticsearch kibana
```

## Integracja z parsedmarc
Po uruchomieniu ELK uruchom parser osobno:
```bash
sudo docker compose up -d parsedmarc
```

W `parsedmarc/parsedmarc.ini` host Elasticsearch powinien wskazywać kontener z tej sieci, np.:
```ini
[elasticsearch]
hosts = es01:9200
ssl = False
```

Uwaga: jeśli używasz wersji TLS/secured Elasticsearch, dostosuj `hosts`, `ssl`, `user`, `password` do swojego środowiska.

## Zatrzymanie
```bash
sudo docker compose -f docker-compose-elk.yml down
```

## Usunięcie danych Elasticsearch
```bash
sudo docker compose -f docker-compose-elk.yml down -v
```
To usunie wolumen `esdata01`.
