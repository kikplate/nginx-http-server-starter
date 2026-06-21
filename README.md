# nginx-http-server-starter

[![View on KikPlate](https://img.shields.io/static/v1?label=KikPlate&message=nginx-http-starter&color=0366d6&style=flat-square)](https://kikplate.dev/plates/nginx-http-starter)

This repository is a Kikplate template for creating a minimal NGINX static HTTP server project with local and container ready configuration.

The template is defined by plate.yaml values.yaml and files in templates with tmpl extensions. You can customize project name ports nginx image and optional modules through values.yaml.

## Build locally from this template

Run this command from the repository root.

```bash
kik generate --template . -f values.yaml --output-dir ./generated-project
```

This generates a project in generated-project using local template files and your current values.

## Generate from Kikplate server

Run this command when using the published template name.

```bash
kik generate nginx-http-starter -f values.yaml
```

This generates the same project shape using the remote template registry source.

## How to use the generated project

Enter the generated project directory then start NGINX with the included config.

```bash
cd generated-project
nginx -c $(pwd)/nginx.conf -p $(pwd)
```

Open http://localhost:8080 in your browser.
