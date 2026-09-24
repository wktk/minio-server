# wktk/minio-server

A MinIO server wrapper to use with GitHub Actions.

https://github.com/wktk/minio-server

> [!NOTE]
> This repository is archived. GitHub Actions service containers now support `command` (and `entrypoint`),
> so a wrapper image is no longer needed.
> See [Communicating with Docker service containers](https://docs.github.com/actions/using-containerized-services/about-service-containers).
>
> The `minio/minio` image is no longer available on Docker Hub. You can use `quay.io/minio/minio` instead.

## Alternative

```yaml
name: minio server example

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    services:
      minio:
        image: quay.io/minio/minio:RELEASE.2025-09-07T16-13-09Z
        ports:
          - 9000:9000
        env:
          MINIO_ROOT_USER: AKIAIOSFODNN7EXAMPLE
          MINIO_ROOT_PASSWORD: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
        command: server /data
        options: >-
          --health-cmd "curl -f http://localhost:9000/minio/health/live"
          --health-interval 5s
          --health-timeout 5s
          --health-retries 10
    env:
      AWS_ACCESS_KEY_ID: AKIAIOSFODNN7EXAMPLE
      AWS_SECRET_ACCESS_KEY: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
      AWS_DEFAULT_REGION: us-east-1
      AWS_ENDPOINT_URL: http://localhost:9000
    steps:
      - run: aws s3 mb s3://test-bucket
      - run: echo hello | aws s3 cp - s3://test-bucket/hello.txt
      - run: aws s3 cp s3://test-bucket/hello.txt - | grep -x hello
```

<details>
<summary>Old usage (wktk/minio-server image)</summary>

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

</details>
