# nginx-http-starter

[![View on KikPlate](https://img.shields.io/static/v1?label=KikPlate&message=nginx-http-starter&color=0366d6&style=flat-square)](https://kikplate.dev/plates/nginx-http-starter)

A minimal NGINX static server starter for local development and Docker deployment.

## Features

- Static file hosting from `public/`
- Native NGINX workflow for local development
- Docker and Docker Compose support
- Gzip enabled in HTTP config
- Cache headers for common static assets
- Clean, small starter structure

## Project Structure

```text
nginx-http-server-starter/
├── .dockerignore
├── .editorconfig
├── .gitignore
├── LICENSE
├── README.md
├── conf.d/
│   ├── active.conf
│   ├── dev.conf
│   └── prod.conf
├── docker-compose.yaml
├── dockerFile
├── mime.types
├── nginx.conf
└── public/
    └── index.html
```

## Quick Start

Use one of the two options below.

## Option A: Native NGINX (Local)

Local NGINX uses `conf.d/active.conf`. By default, this starter keeps `active.conf` aligned with `dev.conf`.

### Start

From the project root:

```bash
nginx -c $(pwd)/nginx.conf -p $(pwd)
```

Open: http://localhost:8080

### Switch Environment

Use one config at a time by copying it into `conf.d/active.conf`.

Development:

```bash
cp conf.d/dev.conf conf.d/active.conf
nginx -t -c $(pwd)/nginx.conf -p $(pwd)
nginx -s reload -c $(pwd)/nginx.conf -p $(pwd)
```

Production:

```bash
cp conf.d/prod.conf conf.d/active.conf
nginx -t -c $(pwd)/nginx.conf -p $(pwd)
nginx -s reload -c $(pwd)/nginx.conf -p $(pwd)
```

Note: `dev.conf` listens on port `8080` and serves from `./public`. `prod.conf` listens on port `80` and serves from `/usr/share/nginx/html`, which is intended for the container image.

### Validate Config

```bash
nginx -t -c $(pwd)/nginx.conf -p $(pwd)
```

### Reload After Config Changes

```bash
nginx -s reload -c $(pwd)/nginx.conf -p $(pwd)
```

### Stop

```bash
nginx -s stop -c $(pwd)/nginx.conf -p $(pwd)
```

## Option B: Docker

Docker always uses the production server config during image build.

### Run With Compose

```bash
docker compose -f docker-compose.yaml up --build
```

Open: http://localhost:8080

### Stop Compose

```bash
docker compose -f docker-compose.yaml down
```

### Build and Run Manually

```bash
docker build -f dockerFile -t nginx-http-starter .
docker run --rm -p 8080:80 nginx-http-starter
```

## Configuration Notes

- Main config: `nginx.conf`
- Active server config: `conf.d/active.conf`
- Development server config: `conf.d/dev.conf`
- Production server config: `conf.d/prod.conf`
- MIME definitions: `mime.types`
- Static content: `public/index.html`

Native mode loads only `conf.d/active.conf`, which prevents duplicate server blocks and avoids conflicting `server_name` warnings during reload.

Docker mode maps host `8080` to container `80` (`docker-compose.yaml`) and the image build copies `prod.conf` to `active.conf` automatically.

## Development Flow

```bash
# 1) Start locally
nginx -c $(pwd)/nginx.conf -p $(pwd)

# 2) Edit files
#    - public/index.html
#    - conf.d/dev.conf or conf.d/prod.conf
#    - conf.d/active.conf
#    - nginx.conf

# 3) Validate and reload
nginx -t -c $(pwd)/nginx.conf -p $(pwd)
nginx -s reload -c $(pwd)/nginx.conf -p $(pwd)

# 4) Stop
nginx -s stop -c $(pwd)/nginx.conf -p $(pwd)
```