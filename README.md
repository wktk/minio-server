# wktk/minio-server

A MinIO server wrapper to use with GitHub Actions.

https://github.com/wktk/minio-server

## Example Usage

```yaml
name: minio server example

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    services:
      minio:
        image: wktk/minio-server
        ports:
          - 9000:9000
        env:
          MINIO_ACCESS_KEY: AKIAIOSFODNN7EXAMPLE
          MINIO_SECRET_KEY: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
    steps:
      - name: Add 127.0.0.1 minio to /etc/hosts (if you need)
        run: echo "127.0.0.1 minio" | sudo tee -a /etc/hosts

      - name: Do something
        run: echo "Fix me!"
```

## Dockerfile

```Dockerfile
FROM minio/minio:latest
CMD ["minio", "server", "/data"]
```
