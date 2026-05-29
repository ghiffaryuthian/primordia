# Primordia

Primordia is a standalone opinionated fullstack project bootstrapper. It asks for a service name, Go module path, target directory, and domain list, then renders a fresh full-stack service scaffold from templates.

## Usage

Interactive:

```sh
/home/ghiffaryuthian/primordia/init-project
```

Repeatable:

```sh
/home/ghiffaryuthian/primordia/init-project \
  --service atlas \
  --module github.com/yourname/atlas \
  --target /home/ghiffaryuthian/atlas \
  --domains "customer, invoice" \
  --force
```

Then in the generated project:

```sh
cd /home/ghiffaryuthian/atlas
git init
pnpm --dir web install
task dev:tools
task telemetry:generate
task proto:generate
task sqlc:generate
go mod tidy
task dev:prepare
```

## Template Layout

- `init-project`: prompts, normalizes names, builds domain-specific replacement blocks, and renders templates.
- `templates/static`: files rendered once per project.
- `templates/domain`: files rendered once per domain.
- `templates/snippets`: small reusable pieces, currently used for migration table/drop blocks.

## Generated Surface

Primordia creates:

- Go API and web server commands
- ConnectRPC proto files for health and each domain
- CRUD domain packages under `internal/<domain>`
- PostgreSQL migrations and sqlc queries for each domain
- Postgres infrastructure and repository implementations
- local Docker Compose files and Dockerfiles
- default telemetry registry YAML, Weaver codegen templates, OTLP metrics, and OTLP tracing setup
- a TanStack Router and TanStack Query web shell with shared Connect clients
