# ns8-postiz

postiz offers everything you need to manage your social media posts, build an audience, capture leads, and grow your business faster with AI

Module id: `postiz`. Images are pinned in `build-images.sh` and updated through reviewed changes.

## Install

```bash
add-module ghcr.io/geniusdynamics/postiz:latest 1
```

The command returns the instance id, for example `postiz1`. Use the id NS8 returns.

## Configure

```bash
api-cli run configure-module --agent module/postiz1 --data - <<'EOF'
{
  "host": "postiz.example.org",
  "http2https": true,
  "lets_encrypt": false
}
EOF
```

| Parameter | Required | Type | Description |
|---|---|---|---|
| `host` | yes | string | Host name for the application, like 'postiz.domain.org' |
| `lets_encrypt` | yes | boolean | Request a valid Let's Encrypt certificate. |
| `http2https` | yes | boolean | Redirect all the HTTP requests to HTTPS |

## Configuration output

`get-configuration` returns: `host`, `lets_encrypt`, `http2https`.

## Images

| Image | Version |
|---|---|
| `ghcr.io/gitroomhq/postiz-app` | `v2.25.3` |
| `ghcr.io/getsentry/spotlight` | `4.12.1` |
| `docker.io/temporalio/ui` | `2.34.0` |
| `docker.io/postgres` | `17.12-alpine3.24` |
| `docker.io/redis` | `7.2.14-alpine` |
| `docker.io/elasticsearch` | `7.17.27` |
| `docker.io/temporalio/auto-setup` | `1.28.1` |
| `docker.io/temporalio/admin-tools` | `1.28.1-tctl-1.18.4-cli-1.4.1` |

## Network and access

- Reserves 3 TCP port(s) on the node. The web application is published through Traefik on the configured host name.
- Runs rootless.
- Declares these NS8 authorizations: `traefik@node:routeadm`.

## Backup

The following paths are included in the NS8 backup (`imageroot/etc/state-include.conf`):

- `state/spotlight.env`
- `state/temporal-ui.env`
- `state/postiz-postgres.env`
- `state/temporal-admin-tools.env`
- `state/postiz.env`
- `state/postiz-app.env`
- `state/postiz-redis.env`
- `state/temporal-elasticsearch.env`
- `state/temporal.env`
- `volumes/postgres-data`
- `volumes/postiz-redis-data`
- `volumes/elasticsearch-data`
- `volumes/dynamicconfig`
- `volumes/postiz-config`
- `volumes/postiz-uploads`

## Services

- `postiz-app.service`
- `postiz-postgres-app.service`
- `postiz-redis-app.service`
- `postiz.service`
- `spotlight-app.service`
- `temporal-admin-tools-app.service`
- `temporal-app.service`
- `temporal-elasticsearch-app.service`
- `temporal-ui-app.service`

## Get the configuration

```bash
api-cli run get-configuration --agent module/postiz1
```

## Update and uninstall

```bash
api-cli run update-module --data '{"module_url":"ghcr.io/geniusdynamics/postiz:latest","instances":["postiz1"],"force":true}'
remove-module --no-preserve postiz1
```

## Testing

Continuous integration:

- `Publish images` builds the module image on every push.
- `Test module` installs the image on a QEMU NS8 node and runs the Robot Framework suite in `tests/`. It runs after `Publish images` succeeds on `dev` and on `main`.
- `Build API JSON Schema docs` and the security scan run alongside.

The suite installs the module, configures a host name, checks that the web application answers through Traefik, takes UI screenshots and removes the module.

Run it locally with the NS8 standard infrastructure described in the [ns8-github-actions README](https://github.com/NethServer/ns8-github-actions/blob/v1/README.md#running-tests-locally).

## Upstream

Application documentation: https://docs.postiz.com/

## License

GPL-3.0-or-later. See [LICENSE](LICENSE).
