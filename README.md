# Build Secure Docker Image

![GitHub Marketplace](https://img.shields.io/badge/marketplace-Build%20Secure%20Docker%20Image-blue?logo=github)

A GitHub Action to **build Docker images** and **scan them with [Dockle](https://github.com/goodwithtech/dockle)** for security best practices.
This helps ensure your container images follow recommended security guidelines before being deployed.

---

## 🚀 Features

* Build a Docker image using a custom or default `docker build` command.
* Automatically fetch and install the latest **Dockle** version (or use a specified one).
* Scan images for **INFO / WARN / FATAL** level issues.
* Fail the workflow if security issues are detected (configurable).

---

## 📦 Usage

```yaml
name: CI - Build and Scan Docker Image

on:
  push:
    branches: [ "main" ]

jobs:
  docker-security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Build & Scan Docker Image
        uses: Pribal/build-secure-docker-image@v1
        with:
          tag: my-app:latest
```

---

## ⚙️ Inputs

| Name                   | Description                                                      | Required | Default                                        |
| ---------------------- | ---------------------------------------------------------------- | -------- | ---------------------------------------------- |
| `tag`                  | Docker image name and tag in `name:tag` format                   | ✅ Yes    | –                                              |
| `path`                 | Directory where the Docker build command should run              | ❌ No     | `.`                                            |
| `dockerfile`           | Path to the Dockerfile relative to the `path` input              | ❌ No     | `Dockerfile`                                   |
| `docker-build-command` | Custom Docker build command (overrides default)                  | ❌ No     | `docker build -f <dockerfile> -t <tag> <path>` |
| `dockle-version`       | Dockle version to use (defaults to latest release)               | ❌ No     | `latest`                                       |
| `dockle-exit-code`     | Exit code returned by Dockle when WARN or FATAL issues are found | ❌ No     | `1`                                            |
| `dockle-exit-level`    | Dockle exit threshold: `INFO`, `WARN`, or `FATAL`                | ❌ No     | `WARN`                                         |

---

## 🔎 Example with Custom Build Command

```yaml
- name: Build & Scan with custom build command
  uses: Pribal/build-secure-docker-image@v1
  with:
    tag: custom-app:1.0.0
    docker-build-command: docker buildx build --platform linux/amd64 -t custom-app:1.0.0 .
```

---

## ✅ Example with Custom Dockle Settings

```yaml
- name: Build & Scan with Dockle settings
  uses: Pribal/build-secure-docker-image@v1
  with:
    tag: secure-app:latest
    dockle-exit-level: FATAL
    dockle-exit-code: 2
    dockle-version: 0.4.13
```

---

## 📝 Notes

* Requires Docker to be available in the runner (tested on `ubuntu-latest`).
* Dockle scan results are printed in the job logs.
* If the scan finds issues at or above the `dockle-exit-level`, the workflow will **fail with the configured exit code**.