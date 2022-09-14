# ddev behind traefik
- replace `BASE_DOMAIN` in `config/dynamic/routers.yml` with base domain name, it will be used as base for project specific subdomians
- configure all ddev projects to use non standard ports for http and https and to use additional custom subdomain, for each project in `<path_to_project>/.ddev/` dir add config override file, for example `config.test.yml` with content:
```yaml
router_https_port: "444"
router_http_port: "8080"
additional_fqdns:
  - <project_subdomain>
```
- `<project_subdomain>` should be replaced with subdomain of `BASE_DOMAIN` usedd in traefik config
  - for example if `BASE_DOMAIN` is `example.com` then project subdomian should be `<project>.example.com`
- make sure to do it for all ddev project and restart ddev for it to bind only custom ports and not default 80/443
- start traefik `docker compose up -d`

For ssl certs to work you need to additionally configure them in traefik config. We used wildcard cert for `BASE_DOMAIN` so there's no need to generate separate certs for each project subdomain.

You can also add middleware in traefik with some authentication and apply it to all routers in `/config/dynamic/routers.yml` to limit external access to projects.
