# DevOps Diploma Application

## Application repository

- Application: https://github.com/aikochnev/diplom-docker
- Infrastructure repository: <ссылка на Terraform repository>
- Container Registry: cr.yandex/<REGISTRY_ID>/my-app

## Architecture

GitHub Actions builds a Docker image, pushes it to Yandex Container Registry,
and deploys the image to two Yandex Compute Cloud VMs over SSH.

```text
GitHub Actions
      |
      v
Yandex Container Registry
      |
      +--> web-b VM
      |
      +--> web-d VM
```

## Local run

```bash
docker compose up -d --build
curl http://localhost:8080
docker compose down
```

## CI/CD

Push to `main` runs `.github/workflows/ci-cd.yml`.

Pipeline steps:

1. Build Docker image
2. Push image to Yandex Container Registry
3. Deploy to web-b
4. Deploy to web-d
5. Verify HTTP response

## Verification

```bash
curl http://<WEB_B_IP>
curl http://<WEB_D_IP>
```

## Security

- SSH deployment key is stored in GitHub Secrets.
- Registry credentials are stored in GitHub Secrets.
- VM retrieves a short-lived IAM token from the metadata service to pull images.
- The Terraform state is stored in Yandex Object Storage.
