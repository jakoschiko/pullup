# pullup

Use `git pull && docker compose up` for all your services!

## Vision

pullup is a tool that deploys your services based on conventions. For each service the conventions are:

- The service is described by a docker-compose project
- The project is configures by a `.env` file
- The project is located in a remote git reposity

pullup runs on your server and can be configured via a single config file:

```toml
[general]
config-refresh = "10s"
service-refresh = "60s"

[service.foo]
git = "https://github.com/example/foo.git"
rev = "main"

[[env]]
name = "user"
value = "admin"

[[env]]
name = "password"
value = "secret"

[service.bar]
git = "https://github.com/example/bar.git"
rev = "deploy"

[[env]]
name = "port"
value = "123"
```

If pullup sees a service config for the first time, then pullup

- clones the repository,
- creates a `.env` file and
- runs `docker compose up`.

If a service config was removed, then pullup

- runs `docker compose down` and
- deletes the repository.

If a service config was changed, then pullup

- runs `docker compose down`,
- deletes the repository,
- clones the repository,
- creates a `.env` file and
- runs `docker compose up`.

For each service pullup periodically

- fetches the repository,
- continues if the revision has changed,
- runs `docker compose down`,
- sets the HEAD to the current revision and
- runs `docker compose up`.

## Existing solutions

TODO
