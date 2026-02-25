# Guía de Deploy

## Flujo automático (normal)

1. Adri hace push a `develop` → deploy automático a **dev**
2. Adri abre PR `develop → main`
3. CodeRabbit revisa + CI corre tests
4. Julio o Carlos aprueban el PR
5. Merge → deploy automático a **prod**

## Deploy manual de emergencia

```bash
# SSH a la VM via IAP
gcloud compute ssh github-actions@erp-prod \
  --zone=southamerica-east1-c \
  --tunnel-through-iap \
  --project=chalar-erp

# SSH directo a dev (nueva zona desde 2026-02-25)
ssh -i /home/lito/.ssh/google_compute_engine lito@34.176.45.29

# En la VM
cd /opt/erp
docker-compose pull
docker-compose up -d
```

## Rollback

```bash
# En la VM, volver a una imagen anterior
docker-compose down
export TAG=<sha-del-commit-anterior>
docker-compose up -d
```

## Monitoreo

- **Health check backend:** `http://[IP]/api/actuator/health`
- **Swagger UI:** `http://[IP]/swagger-ui.html`
- **Logs:** `docker-compose logs -f backend`
