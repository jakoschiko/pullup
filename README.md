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
repositories = "~/pullup"
config-refresh = "10s"
repository-refresh = "60s"
container-refresh = "02:00"

[[service]]
name = "foo"
git = "https://github.com/example/foo.git"
rev = "main"

[[service.env]]
name = "user"
value = "admin"

[[service.env]]
name = "password"
value = "secret"

[[service]]
name = "bar"
git = "https://github.com/example/bar.git"
rev = "deploy"

[[service.env]]
name = "port"
value = "123"
```

If pullup sees a service config for the first time, then pullup

- clones the repository,
- creates a `.env` file,
- runs `docker compose pull` and
- runs `docker compose up --force-recreate`.

If a service config was removed, then pullup

- runs `docker compose down` and
- deletes the repository.

If a service config was changed, then pullup

- runs `docker compose down`,
- deletes the repository,
- clones the repository,
- creates a `.env` file,
- runs `docker compose pull` and
- runs `docker compose up --force-recreate`.

For each service pullup periodically

- fetches the repository,
- continues if the revision has changed,
- runs `docker compose down`,
- sets the HEAD to the current revision and
- runs `docker compose up`.

For each service pullup periodically

- runs `docker compose pull` and
- continues if an image has changed,
- runs `docker compose up --force-recreate`.

## Open questions

- Do we want to run `docker system prune --all` periodically? Or something similar for cleanup?

## Existing solutions

- Kubernetes and clones
  - Too powerful and complex for my simple use-case
- [Harbormaster](https://gitlab.com/stavros/harbormaster)
  - Looks very promising
  - Based on docker-compose and git repositories
  - Doesn't seem to do `docker compose pull`
- [Watchtower](https://github.com/containrrr/watchtower)
  - Looks very simple
  - Based on simple docker containers
  - Maybe can be combined with other tools
- [Dokku](https://dokku.com/docs/getting-started/installation/)
  - More complex because it's a whole platform
  - Complexity might be worth it
- [Yacht](https://yacht.sh/)
  - Container management web UI
  - Supports container and docker-compose
  - Supports templates and one-click installations
  - Git repository?

## Useful links

- [GitHub deploy keys](https://github.blog/news-insights/product-news/read-only-deploy-keys/)
